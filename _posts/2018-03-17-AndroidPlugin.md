---
layout: post
title: AndroidPlugin
publised: false
categroy: [Transform, ASM]
---

Android编译期代码插桩分为plugin，Transform，修改
## 初始化
AndroidPlugin开发模式三种:
1. build.gradle直接写groovy代码
2. 项目内建buildsrc，调用脚本
3. 独立module打包上传到maven

这里介绍第二种，仅项目内使用。
1. 新建Android项目，新建Module，名字只能为BuildSrc，建好后保留src/main目录和build.gradle
文件，其余全部删除。
2. 创建src/main/groovy目录，该目录为groovy源码目录。删除build.gradle文件全部内容，添加
  >apply plugin: 'groovy'

同步后groovy目录被AS识别为源码目录，可以创建包和类文件。
3. 创建WBCodePlugin.groovy文件
>      package com.sina.weibo.codeplugin
        import org.gradle.api.Plugin
        import org.gradle.api.Project
        public class WBCodePlugin implements Plugin<Project> {
        @Override
        void apply(Project project) {
          project.task('testPlugin') << {
            println "Hello gradle plugin in src"
          }
          }
        }

配置app的build.gradle文件，注意不能加单引号，这里调用的是插件类本身
apply plugin: com.sina.weibo.codeplugin.WBCodePlugin
运行clean 查看gradle控制台就有想要的输出了。

4. app的build.gradle文件中添加Transform API依赖
>apply plugin: 'groovy'
dependencies {
    compile 'com.android.tools.build:gradle:3.0.0'
    compile gradleApi()
    compile localGroovy()
    compile 'com.android.tools.build:transform-api:1.5.0' //1.5.0稳定版
    compile 'org.ow2.asm:asm:5.0.3' //asm框架
    compile 'org.ow2.asm:asm-commons:5.0.3' //asm框架
    compile 'commons-io:commons-io:2.5'
}
repositories {
    jcenter()
    maven { url 'https://maven.google.com' }
}

5. WBCodePlugin实现Transform接口
>class WBAopPlugin extends Transform implements Plugin<Project> {
    @Override
    void apply(Project project) {
//        project.task('testPlugin') << {
//            println "插件方案"
//        }
        System.out.println("111111%%%%%%%%%%%%%%%%")
//在plugin中注册该Transform，注意AppExtension就是app中build.gradle中android块
//        遍历class文件和jar文件，在这里可以进行class文件asm文件替换
        def android = project.extensions.getByType(AppExtension);
        android.registerTransform(this)
    }
    //实现所有Transform接口方法
    @Override
    void transform(Context context, Collection<TransformInput> inputs, Collection<TransformInput>
            referencedInputs, TransformOutputProvider outputProvider, boolean isIncremental) throws
            IOException, TransformException, InterruptedException {
            //TODO 实现ClassVisitor进行ASM字节码插桩
            }
  }

6. 重写ClassVisitor的visitMethod
>@Override
    public MethodVisitor visitMethod(int access, String name, String desc, String signature,
                                     String[] exceptions) {
        MethodVisitor methodVisitor = cv.visitMethod(access, name, desc, signature, exceptions);
        if (!inject) {
            return methodVisitor;
        }
        methodVisitor = new AdviceAdapter(Opcodes.ASM5, methodVisitor, access, name, desc) {
            private boolean isInject(){
                return inject && InjectManager.isInjectMethod(name, desc);
            }
            @Override
            protected void onMethodEnter() {
                if(isInject()){
                    Utils.print(mv, "onMethodEnter: " + name);
                    mv.visitVarInsn(ALOAD, 1);
                    mv.visitMethodInsn(INVOKESTATIC, InjectManager.INJECT_CLASS_NAME, InjectManager.
                                    INJECT_METHOD_NAME, InjectManager.INJECT_METHOD_DESC, false);
                }
            }
            @Override
            protected void onMethodExit(int i) {
                //super.onMethodExit(i);
                if(isInject()){
                    Utils.print(mv, "onMethodExit: " + name);
                }
            }
        };
        return methodVisitor;
    }

7. 字节码编写可以使用jclasslib这个AS插件查看现有java类生成的class代码
> 0 getstatic #5 <java/lang/System.out>
 3 ldc #8 <start HelloWorld.java onClick>
 5 invokevirtual #7 <java/io/PrintStream.println>
 8 aload_1 //将index为1的local var放到栈顶
 9 invokestatic #9 <com/sina/weibo/aopdemo/HelloCodeTest.click> //调用静态方法
12 return
