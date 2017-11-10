---
layout:     post
title:      "动画案例及设计模式研究"
subtitle:   "Unity 3D学习记录"
date:       2017-11-10
author:     "tomylee"
header-img: "img/contact-bg.jpg"
tags:
    - Unity 3D
---

### 一、设计模式主要分为三类：1. 创建者模式2.结构性模式3.行为模式 
![这里写图片描述](http://img.blog.csdn.net/20170424202200072?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### <一>创建者模式是创建型模式中最负责的一个设计模式了，创建者负责构建一个对象的各个部分，并且完成组装的过程，我们可以这么理解创建者模式，创建者模式类似与一个步骤基本固定，但是每个步骤中的具体形式却又可以变化的这类对象的创建。创建者模式主要是用于创建复杂的一些对象，这些对象的创建步骤基本固定，但是可能具体的对象的组成部分却又可以自由的变化。
#### <二>结构型设计模式是从程序的结构上解决模块之间的耦合问题。包含以下七种模式：
```
(1)适配器模式：Adapter模式通过类的继承或者对象的组合侧重于转换已有的接口，类适配器采用“多继承”的实现方式，带来了不良的高耦合，所以一般不推荐使用。对象适配器采用“对象组合”的方式，更符合松耦合精神。
(2)桥接模式：将抽象部分与实现部分分离，使它们都可以独立的变化。减少因变化带来的代码的修改量。
(3)组合模式：将对象组合成树形结构以表示“部分-整体”的层次结构。Composite模式使得客户对单个对象和组合对象的使用具有一致性。从而解决了解决客户程序与复杂对象容器的解耦，即：通过继承统一的接口，我们可以将容器对象及其子对象看成同一类对象使用，以减少对象使用中的复杂度。
(4)装饰模式：动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。Decorator模式采用对象组合而非继承的手法，实现了在运行时动态的扩展对象功能的能力，而且可以根据需要扩展多个功能，避免了单独使用继承带来的“灵活性差”和“多子类衍生问题”。同时它很好地符合面向对象设计原则中“优先使用对象组合而非继承”和“开放-封闭”原则。
(5)外观模式：为子系统中的一组接口提供一个一致的界面，简化接口。
(6)享元模式：运用共享技术有效地支持大量细粒度的对象。 解决：　面向对象的思想很好地解决了抽象性的问题，一般也不会出现性能上的问题。但是在某些情况下，对象的数量可能会太多，从而导致了运行时的代价。那么我们如何去避免大量细粒度的对象，同时又不影响客户程序使用面向对象的方式进行操作，享元模式的出现恰好解决了该问题。
(7)代理模式：为其他对象提供一种代理以控制这个对象的访问。解决直接访问某些对象是出现的问题。
```
#### <三>行为型模式设计到算法和对象间的职责分配，不仅描述对象或类的模式，还描述它们之间的通信方式，刻划了运行时难以跟踪的复杂的控制流，它们将你的注意力从控制流转移到对象间的关系上来。行为型类模式采用继承机制在类间分派行为，例如Template Method 和Interpreter；行为对象模式使用对象复合而不是继承。一些行为对象模式描述了一组相互对等的对象如何相互协作以完成其中任何一个对象都单独无法完成的任务，如Mediator、Chain of Responsibility、Strategy；其它的行为对象模式常将行为封装封装在一个对象中，并将请求指派给它。常见行为型模式有11种：CCIIMM（Chain of Responsibility职责链、Command命令、Interpreter解释器、Iterator迭代、Mediator中介者、Memento备忘录），OSSTV（Observer观察者、State状态、Strategy策略、Template Method模版方法、Visitor访问者）。
 

### 二、设计模式之间的关系如下：

![这里写图片描述](http://img.blog.csdn.net/20170424202213431?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
【设计模式图片来自：Jaiky_杰哥 
图片出处：http://blog.csdn.net/jaikydota163/article/details/52767394】
#### 接口隔离原则总结：客户端不应该依赖它不需要的接口；一个类对另一个类的依赖应该建立在最小的接口上。 将臃肿的接口I拆分为独立的几个接口，类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则。如果接口过于臃肿，只要接口中出现的方法，不管对依赖于它的类有没有用处，实现类中都必须去实现这些方法，这显然不是好的设计。如果将这个设计修改为符合接口隔离原则，就必须对接口进行拆分。修接口隔离原则的含义是：建立单一接口，不要建立庞大臃肿的接口，尽量细化接口，接口中的方法尽量少。也就是说，我们要为各个类建立专用的接口，而不要试图去建立一个很庞大的接口供所有依赖它的类去调用。在程序设计中，依赖几个专用的接口要比依赖一个综合的接口更灵活。接口是设计时对外部设定的“契约”，通过分散定义多个接口，可以预防外来变更的扩散，提高系统的灵活性和可维护性。
### 三、动画案例一    盖伦击球游戏
游戏案例及所用图片来自：http://blog.csdn.net/qq_28057541/article/details/51172733
![这里写图片描述](http://img.blog.csdn.net/20170424202921127?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 这个人做的是一个盖伦击球动画，其中AnimationLegacy用来控制动作的，和监听鼠标键盘，这里和后面的Judge一起形成了观察者模式。
#### 对象树如下：
![这里写图片描述](http://img.blog.csdn.net/20170424203534714?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 控制动作的AnimationLegacy部分如下：
```
using UnityEngine;
using System.Collections;
using Com.mygame;

public class AnimationLegacy : MonoBehaviour {

    //define animation control object here
    private Animation ani;
    private AnimationState attack;
    private AnimationState idle;
    private AnimationState run;

    public bool isAttacking = false;

    private SenceController sc;

    //用static定义一个全局静态时间
    public delegate void AttackHandler(GameObject obj);
    public static event AttackHandler AttackEvent; 

    // Use this for initialization
    void Start () {
        ani = GetComponent<Animation> ();
        attack = ani ["Attack1"];
        idle = ani["Idle"];
        run = ani["Run"];

        idle.wrapMode = WrapMode.Loop;
        ani.Play (idle.clip.name);

        sc = SenceController.getInstance ();
    }

    // Update is called once per frame
    void Update () {
        float speed = 10f;
        // move
        if (Input.GetKey(KeyCode.W)) {
            //用键盘控制
            transform.Translate (Vector3.forward * Time.deltaTime * speed);
            sc.setIsAttack (false);
            ani.CrossFade ("Run");
        } else if (Input.GetKey(KeyCode.A)) {
            transform.Translate (Vector3.left * Time.deltaTime * speed);
            sc.setIsAttack (false);
            ani.CrossFade ("Run");
        } else if (Input.GetKey (KeyCode.S)) {
            transform.Translate (Vector3.forward * Time.deltaTime * (-speed));
            sc.setIsAttack (false);
            ani.CrossFade ("Run");
        } else if (Input.GetKey(KeyCode.D)) {
            transform.Translate (Vector3.right * Time.deltaTime * speed);
            sc.setIsAttack (false);
            ani.CrossFade ("Run");
        }
        // attck
        if (Input.GetMouseButtonDown(0)) {
            if (sc.getstate () == 0) {
                sc.setstate (1);
                sc.setTimes (Time.time);
            }
            Attack ();
        }
        // if no ani is playing ,then play idle
        if (ani.isPlaying == false) {
            ani.CrossFade ("Idle");
        }

    }

    void Attack() {
        if (attack.clip.events.Length == 0) {
            AnimationEvent endEvent = new AnimationEvent();
            endEvent.functionName = "Attacked";
            endEvent.time = attack.clip.length-0.2f;
            attack.clip.AddEvent (endEvent);
            //Add when is attacking function
            AnimationEvent doingEvent = new AnimationEvent();
            doingEvent.functionName = "Attacking";
            doingEvent.time = 0.5f;
            attack.clip.AddEvent (doingEvent);
            //Add when attck is done
        }
        ani.Play (attack.clip.name);
    }

    void Attacking() {
        sc.setIsAttack (true);
    }

    void Attacked() {
        sc.setIsAttack (false);
    }
    //使用碰撞器实现了打击的判断，添加刚体和碰撞器，在Bollfactory中，对于每一个生成的boll都有添加，同时去掉重力，防止下落
    void OnTriggerStay(Collider c) {
            AttackEvent (c.gameObject);
    }

}
```
### 四、动画案例二 
案例及图片来自: http://dna.yiihuu.com/30793.html
#### Mecanim动画系统的重定向特性，《仙剑奇侠传》是一部经典的RPG游戏，这部游戏到今天依然焕发着强大的生命力，该游戏基于Unity3D，由于技术上的一致性，这个博主向他们索取了一些游戏素材，而这成为了博主决心要研究Mecanim动画系统的一个主要原因。如图是《仙剑奇侠传五前传》中瑕的一个模型：
![这里写图片描述](http://img.blog.csdn.net/20170424204217818?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170424204542775?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 将一个动画片段分割成了4个动画片段，这样就可以通过Animation组件来实现对动画的播放。而这正是在Mecanim动画系统推出之前Unity3D采用的动画控制形式。由于这个模型在建模的时候存在问题，所以导致瑕在游戏场景中的角度出现错误。用3D建模软件进行调整，结果意外得发现瑕的模型中是有骨骼的，可以利用Mecanim动画系统来为这个模型添加动画。由于这个模型在建模的时候将动画和模型一起创建了，因此需要首先将这个动画从模型中去除，因为Mecanim动画系统的一个主要思路就是让一套动画可以通过重定向应用到不同的模型上，既然有动画可用，那么模型自带的动画可以暂时去除。而让动画从模型中去除的方法很简单，就是在导出FBX模型的时候将嵌入的媒体选项不要勾选，这样模型就可以和动画分离开了。
#### AnimatorController：如果说Avater是将模型的身体和骨骼实现匹配的接口，那么AnimatorController就是讲动画和模型实现绑定的接口。创建一个XiaAnimaterController。双击该文件，会打开Animator窗口，如图：
![这里写图片描述](http://img.blog.csdn.net/20170424204435038?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 控制人物切换动画代码如下：
![这里写图片描述](http://img.blog.csdn.net/20170424204658054?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170424204637460?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 最后的动画效果：
![这里写图片描述](http://img.blog.csdn.net/20170424203930301?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170424204018395?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

