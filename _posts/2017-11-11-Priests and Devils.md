---
layout:     post
title:      "Priests and Devils"
subtitle:   "Unity 3D小游戏"
date:       2017-11-11
author:     "tomylee"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Unity 3D
    - C#
---

## Priests and Devils
#### Priests and Devils is a puzzle game in which you will help the Priests and Devils to cross the river within the time limit. There are 3 priests and 3 devils at one side of the river. They all want to get to the other side of this river, but there is only one boat and this boat can only carry two persons each time. And there must be one person steering the boat from one side to the other side. In the flash game, you can click on them to move them and click the go button to move the boat to the other direction. If the priests are out numbered by the devils on either side of the river, they get killed and the game is over. You can try it in many ways. Keep all priests alive! Good luck!
#### Please click here to play the game[Priests and Devils](http://www.flash-game.net/game/2535/priests-and-devils.html)
### 1.列出游戏中提及的事物（Objects） 
答：这个游戏中的事物有:
>游戏角色：3个牧师、3个恶魔；
>
>游戏场景：2个草地河岸、一艘小船；
>
>游戏控件：开始、运动、重试按钮，时间提示；

### 2.请使用课件架构图编程，不接受非 MVC 结构程序 
答：使用三个文件实现MVC架构设计，分别为：
>Basecode.cs               ______ V
>
>
>GenGameObjects          ______     M
>
>SceneController           ______   C
        
![这里写图片描述](http://img.blog.csdn.net/20170330234748587?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 3.在 GenGameObjects 中创建 长方形、正方形、球 及其色彩代表游戏中的对象。 
答：Devil: 恶魔
    Priset:牧师
    Shore:河岸
    Boat:船
   创建效果如下图：
   ![这里写图片描述](http://img.blog.csdn.net/20170330234923802?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM0NTQxMTI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
### 4.用表格列出玩家动作表（规则表），注意，动作越少越好 
	
| 项目        | 条件   | 动作  |
| --------   | -----  | :----  |
|   开船  | 船在开始岸、船在结束岸 |  船开到对面岸     |
| 船的左方/右方下船       | 船靠岸且船左方/右方有对象   |  左边/右边对象下船上岸   |
| 左边/右边岸上船        |   船在左边/右边岸，船有空位，岸上有对象  |  对象坐上船左方/右方  |
|判断结束|任意一边岸与船上恶魔数量大于牧师	|游戏结束|
### 5.实验代码
在上述设计后对内容进行了修改，不再使用预设，而是代码动态生成
##### Action.cs
```c#
using UnityEngine;
using System.Collections;
using Com.Mygame;

public class Actions : MonoBehaviour
{
    GameSceneController mygame;
    ImyAction actions;
    float BottonW = Screen.width / 4;
    float BottonH = Screen.height / 16;
    float firstBx = Screen.width / 4;
    float firstBy = Screen.height / 8;
    float blank = 5;
    private string str = "点击重置！";
    // Use this for initialization
    void Start()
    {

        mygame = GameSceneController.GetInstance();
        actions = GameSceneController.GetInstance() as ImyAction;
    }

    void OnGUI()
    {

        if (GUI.Button(new Rect(firstBx, firstBy, BottonW, BottonH), "魔鬼上船"))
        {
            actions.DevilGoOnBoat();
        }
        if (GUI.Button(new Rect(firstBx + BottonW + blank, firstBy, BottonW, BottonH), "魔鬼下船"))
        {
            actions.DevilGoOffBoat();
        }
        if (GUI.Button(new Rect(firstBx, firstBy + BottonH + blank, BottonW, BottonH), "牧师上船"))
        {
            actions.PriestGoOnBoat();
        }
        if (GUI.Button(new Rect(firstBx + BottonW + blank, firstBy + BottonH + blank, BottonW, BottonH), "牧师下船"))
        {
            actions.PriestGoOffBoat();
        }
        if (GUI.Button(new Rect(firstBx, firstBy + (BottonH + blank) * 2, BottonW * 2 + blank, BottonH * 2), "开船！"))
        {
            actions.BoatGo();
            if (mygame.wl == WinOrLose.Gaming) str = "点击重置！";
            else if (mygame.wl == WinOrLose.Lose) str = "输了...重来！";
            else if (mygame.wl == WinOrLose.Win) str = "赢了！再来！";
        }

        if (GUI.Button(new Rect(Screen.width - BottonW / 2, firstBy, BottonW / 2, BottonH), str))
        {
            Application.LoadLevel(Application.loadedLevelName);
            mygame.wl = WinOrLose.Gaming;
        }

        //GUI.Label (new Rect (firstBx,firstBy-BottonH,BottonW*2,BottonH), str);

    }
}
```

##### BaseCode.cs
```c#
using UnityEngine;
using System.Collections;
using Com.Mygame;

namespace Com.Mygame
{

    public enum WinOrLose { Win, Lose, Gaming };

    public interface ImyAction
    {
        void DevilGoOnBoat();
        void DevilGoOffBoat();
        void PriestGoOnBoat();
        void PriestGoOffBoat();
        void BoatGo();
    }

    public class GameSceneController : System.Object, ImyAction
    {
        private static GameSceneController _instance;
        private BaseCode _base_code;
        private GenGameObjects _gen_game_object;

        public WinOrLose wl = WinOrLose.Gaming;

        public static GameSceneController GetInstance()
        {
            if (null == _instance)
            {
                _instance = new GameSceneController();
            }
            return _instance;
        }

        public BaseCode getBaseCode()
        {
            return _base_code;
        }

        internal void setBaseCode(BaseCode bc)
        {
            if (null == _base_code)
            {
                _base_code = bc;
            }
        }

        public GenGameObjects getGenGameObject()
        {
            return _gen_game_object;
        }

        internal void setGenGameObject(GenGameObjects g)
        {
            if (_gen_game_object == null)
            {
                _gen_game_object = g;
            }
        }

        public void DevilGoOnBoat()
        {
            _gen_game_object.DevilGoOnBoat();
        }

        public void DevilGoOffBoat()
        {
            _gen_game_object.DevilGoOffBoat();
        }

        public void PriestGoOnBoat()
        {
            _gen_game_object.PriestGoOnBoat();
        }

        public void PriestGoOffBoat()
        {
            _gen_game_object.PriestGoOffBoat();
        }

        public void BoatGo()
        {
            _gen_game_object.BoatGo();
        }
    }
}

public class BaseCode : MonoBehaviour
{

    public string gameName;
    public string gameRule;


    // Use this for initialization
    void Start()
    {
        this.transform.position = new Vector3(0.0f, 5.0f, -12.0f);
        GameSceneController mygame = GameSceneController.GetInstance();
        mygame.setBaseCode(this);
    }

    // Update is called once per frame
    void Update()
    {

    }
}
```

##### GenGameObject.cs
```c#
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using Com.Mygame;

public class GenGameObjects : MonoBehaviour
{

    GameSceneController mygame;
    //boat
    int state = 0;
    GameObject Boat;
    Vector3 boatscale = new Vector3(4.0f, 0.3f, 2.0f);  //boat's size;
    Vector3 boatStartpos = new Vector3(-6.0f, 0.0f, 0.0f);
    Vector3 boatEndpos = new Vector3(6.0f, 0.0f, 0.0f);
    // sits on the boat
    Vector3 boatsit1 = new Vector3(-0.25f, 3.45f, 0.0f); //  localposition is care with the scale
    Vector3 boatsit2 = new Vector3(0.25f, 3.45f, 0.0f);
    // others about boats
    float boatspeed = 1.0f;
    int boatnumber = 0;
    int Ifboatsit1have = 0;
    int Ifboatsit2have = 0;
    //river


    //Devils
    List<GameObject> devils = new List<GameObject>();
    Vector3[] devilStartpos = new Vector3[3]{
        new Vector3(-10f,1.15f,-3f),
        new Vector3(-10f,1.15f,-2f),
        new Vector3(-10f,1.15f,-1f)
    };
    Vector3[] devilEndpos = new Vector3[3]{
        new Vector3(10f,1.15f,-3f),
        new Vector3(10f,1.15f,-2f),
        new Vector3(10f,1.15f,-1f)
    };
    Vector3[] devilScale = new Vector3[3]{
        new Vector3(),
        new Vector3(),
        new Vector3()
    };
    int[] DevilsState = new int[3] { 0, 0, 0 }; // 0-startside 1-endside 2-onboat
    int[] Devilssite = new int[3] { 0, 0, 0 }; //0表示 nosite 1-1site 2-2site

    int devilStartside = 3;
    int devilEndside = 0;

    //Priest
    List<GameObject> priests = new List<GameObject>();
    Vector3[] priestStartpos = new Vector3[3]{
        new Vector3(-10f,1.15f,1f),
        new Vector3(-10f,1.15f,2f),
        new Vector3(-10f,1.15f,3f)
    };
    Vector3[] prisetEndpos = new Vector3[3]{
        new Vector3(10f,1.15f,1f),
        new Vector3(10f,1.15f,2f),
        new Vector3(10f,1.15f,3f)
    };
    Vector3[] priestScale = new Vector3[3]{
        new Vector3(),
        new Vector3(),
        new Vector3()
    };

    int[] PriestState = new int[3] { 0, 0, 0 }; // 0-startside 1-endside 2-onboat
    int[] Priestsite = new int[3] { 0, 0, 0 };

    int priestStartside = 3;
    int priestEndside = 0;

    // startside
    GameObject startside;
    Vector3 startsidepos = new Vector3(-10.0f, 0.0f, 0.0f);
    Vector3 startsidescale = new Vector3(4f, 0.3f, 8f);
    // endside
    GameObject endside;
    Vector3 endsidepos = new Vector3(10.0f, 0.0f, 0.0f);
    Vector3 endsidescale = new Vector3(4f, 0.3f, 8f);

    // Use this for initialization
    void Start()
    {
        mygame = GameSceneController.GetInstance();
        mygame.setGenGameObject(this);
        InsAll();
    }

    // Update is called once per frame
    void Update()
    {

        float step = boatspeed * Time.deltaTime;
        if (state == 2)
        {
            Boat.transform.position = Vector3.MoveTowards(Boat.transform.position, boatEndpos, boatspeed);
            if (Boat.transform.position == boatEndpos) state = 1;
        }
        else if (state == 3)
        {
            Boat.transform.position = Vector3.MoveTowards(Boat.transform.position, boatStartpos, boatspeed);
            if (Boat.transform.position == boatStartpos) state = 0;
        }
    }



    // ins all gameobjects
    void InsAll()
    {
        //sides
        startside = GameObject.CreatePrimitive(PrimitiveType.Cube);
        startside.transform.localScale = startsidescale;
        startside.transform.position = startsidepos;
        startside.GetComponent<Renderer>().material.color = Color.green;
        endside = GameObject.CreatePrimitive(PrimitiveType.Cube);
        endside.transform.localScale = endsidescale;
        endside.transform.position = endsidepos;
        endside.GetComponent<Renderer>().material.color = Color.green;
        //boat
        Boat = GameObject.CreatePrimitive(PrimitiveType.Cube);
        Boat.transform.position = boatStartpos;
        Boat.transform.localScale = boatscale;
        Boat.GetComponent<Renderer>().material.color = Color.grey;
        //devils and Priests
        for (int i = 0; i < 3; i++)
        {
            GameObject devil = GameObject.CreatePrimitive(PrimitiveType.Cylinder);
            devil.GetComponent<Renderer>().material.color = Color.red;
            devil.transform.position = devilStartpos[i];
            devils.Add(devil);
            GameObject priest = GameObject.CreatePrimitive(PrimitiveType.Cylinder);
            priest.transform.position = priestStartpos[i];
            priests.Add(priest);

        }

        //light !!cool
        GameObject light = new GameObject("The light");
        Light lightComp = light.AddComponent<Light>();
        lightComp.type = LightType.Directional;
        light.transform.position = new Vector3();
        Quaternion Q;
        Q = Quaternion.Euler(20.0f, 0.0f, 0.0f);
        light.transform.rotation = Q;

    }

    public void DevilGoOnBoat()
    { // must write by public ,or the interface can not use it
        if (state == 0)
        { //startside
            if (boatnumber != 2)
            {
                for (int i = 0; i < 3; i++)
                {
                    if (DevilsState[i] == 0)
                    {

                        DevilsState[i] = 2;//mean now this devil's state is on boat

                        devils[i].transform.parent = Boat.transform;

                        if (Ifboatsit1have == 0 && Ifboatsit2have == 0)
                        {
                            devils[i].transform.localPosition = boatsit1;
                            Devilssite[i] = 1;
                            Ifboatsit1have = 1;
                        }
                        else if (Ifboatsit1have == 1 && Ifboatsit2have == 0)
                        {
                            devils[i].transform.localPosition = boatsit2;
                            Devilssite[i] = 2;
                            Ifboatsit2have = 1;
                        }
                        else if (Ifboatsit1have == 0 && Ifboatsit2have == 1)
                        {
                            devils[i].transform.localPosition = boatsit1;
                            Devilssite[i] = 1;
                            Ifboatsit1have = 1;
                        }

                        devilStartside--;
                        boatnumber++;
                        break;
                    }
                }
            }
        }
        else if (state == 1)
        { // endside
            if (boatnumber != 2)
            {
                for (int i = 0; i < 3; i++)
                {
                    if (DevilsState[i] == 1)
                    {

                        DevilsState[i] = 2;//mean now this devil's state is on boat
                        devils[i].transform.parent = Boat.transform;

                        if (Ifboatsit1have == 0 && Ifboatsit2have == 0)
                        {
                            devils[i].transform.localPosition = boatsit1;
                            Devilssite[i] = 1;
                            Ifboatsit1have = 1;
                        }
                        else if (Ifboatsit1have == 1 && Ifboatsit2have == 0)
                        {
                            devils[i].transform.localPosition = boatsit2;
                            Devilssite[i] = 2;
                            Ifboatsit2have = 1;
                        }
                        else if (Ifboatsit1have == 0 && Ifboatsit2have == 1)
                        {
                            devils[i].transform.localPosition = boatsit1;
                            Devilssite[i] = 1;
                            Ifboatsit1have = 1;
                        }

                        devilEndside--;
                        boatnumber++;
                        break;
                    }
                }
            }
        }
    }

    public void DevilGoOffBoat()
    {
        if (state == 0)
        { // startside
            for (int i = 0; i < 3; i++)
            {
                if (DevilsState[i] == 2)
                {
                    devils[i].transform.parent = null;
                    devils[i].transform.position = devilStartpos[i];
                    DevilsState[i] = 0;

                    if (Devilssite[i] == 1)
                        Ifboatsit1have = 0;
                    else if (Devilssite[i] == 2)
                        Ifboatsit2have = 0;
                    Devilssite[i] = 0;

                    devilStartside++;
                    boatnumber--;
                    break;
                }
            }
        }
        else if (state == 1)
        { // endside
            for (int i = 0; i < 3; i++)
            {
                if (DevilsState[i] == 2)
                {
                    devils[i].transform.parent = null;
                    devils[i].transform.position = devilEndpos[i];
                    DevilsState[i] = 1;

                    if (Devilssite[i] == 1)
                        Ifboatsit1have = 0;
                    else if (Devilssite[i] == 2)
                        Ifboatsit2have = 0;
                    Devilssite[i] = 0;

                    devilEndside++;
                    boatnumber--;
                    break;
                }
            }
        }
    }

    public void PriestGoOnBoat()
    {
        if (state == 0)
        { //startside
            if (boatnumber != 2)
            {
                for (int i = 0; i < 3; i++)
                {
                    if (PriestState[i] == 0)
                    {

                        PriestState[i] = 2;//mean now this devil's state is on boat
                        priests[i].transform.parent = Boat.transform;

                        if (Ifboatsit1have == 0 && Ifboatsit2have == 0)
                        {
                            priests[i].transform.localPosition = boatsit1;
                            Priestsite[i] = 1;
                            Ifboatsit1have = 1;
                        }
                        else if (Ifboatsit1have == 1 && Ifboatsit2have == 0)
                        {
                            priests[i].transform.localPosition = boatsit2;
                            Priestsite[i] = 2;
                            Ifboatsit2have = 1;
                        }
                        else if (Ifboatsit1have == 0 && Ifboatsit2have == 1)
                        {
                            priests[i].transform.localPosition = boatsit1;
                            Priestsite[i] = 1;
                            Ifboatsit1have = 1;
                        }

                        priestStartside--;
                        boatnumber++;
                        break;
                    }
                }
            }
        }
        else if (state == 1)
        { // endside
            if (boatnumber != 2)
            {
                for (int i = 0; i < 3; i++)
                {
                    if (PriestState[i] == 1)
                    {

                        PriestState[i] = 2;//mean now this devil's state is on boat
                        priests[i].transform.parent = Boat.transform;

                        if (Ifboatsit1have == 0 && Ifboatsit2have == 0)
                        {
                            priests[i].transform.localPosition = boatsit1;
                            Priestsite[i] = 1;
                            Ifboatsit1have = 1;
                        }
                        else if (Ifboatsit1have == 1 && Ifboatsit2have == 0)
                        {
                            priests[i].transform.localPosition = boatsit2;
                            Priestsite[i] = 2;
                            Ifboatsit2have = 1;
                        }
                        else if (Ifboatsit1have == 0 && Ifboatsit2have == 1)
                        {
                            priests[i].transform.localPosition = boatsit1;
                            Priestsite[i] = 1;
                            Ifboatsit1have = 1;
                        }

                        priestEndside--;
                        boatnumber++;
                        break;
                    }
                }
            }
        }
    }

    public void PriestGoOffBoat()
    {
        if (state == 0)
        { // startside
            for (int i = 0; i < 3; i++)
            {
                if (PriestState[i] == 2)
                {
                    priests[i].transform.parent = null;
                    priests[i].transform.position = priestStartpos[i];
                    PriestState[i] = 0;
                    if (Priestsite[i] == 1)
                        Ifboatsit1have = 0;
                    else if (Priestsite[i] == 2)
                        Ifboatsit2have = 0;
                    Priestsite[i] = 0;
                    priestStartside++;
                    boatnumber--;
                    break;
                }
            }
        }
        else if (state == 1)
        { // endside
            for (int i = 0; i < 3; i++)
            {
                if (PriestState[i] == 2)
                {
                    priests[i].transform.parent = null;
                    priests[i].transform.position = prisetEndpos[i];
                    PriestState[i] = 1;

                    if (Priestsite[i] == 1)
                        Ifboatsit1have = 0;
                    else if (Priestsite[i] == 2)
                        Ifboatsit2have = 0;
                    Priestsite[i] = 0;
                    priestEndside++;
                    boatnumber--;
                    break;
                }
            }
        }
    }

    public void BoatGo()
    {
        if (boatnumber != 0)
        {
            check();
            if (state == 0)
            {
                // from startpos to endpos,move
                state = 2;
            }
            else if (state == 1)
            {
                state = 3;
                // form endpos to startpos,move
            }
        }
    }

    public void check()
    {
        //lose check
        print(devilStartside);
        print(devilEndside);
        print(priestStartside);
        print(priestEndside);
        if (state == 0)
        {
            if ((devilStartside > priestStartside && priestStartside != 0) || ((3 - devilStartside) > (3 - priestStartside) && (3 - priestStartside) != 0))
            {
                mygame.wl = WinOrLose.Lose;
                return;
            }
        }
        else if (state == 1)
        {
            if ((devilEndside > priestEndside && priestEndside != 0) || ((3 - devilEndside) > (3 - priestEndside) && (3 - priestEndside) != 0))
            {
                mygame.wl = WinOrLose.Lose;
                return;
            }
        }
        //win check
        if (devilStartside == 0 && priestStartside == 0) mygame.wl = WinOrLose.Win;

    }
}

```
