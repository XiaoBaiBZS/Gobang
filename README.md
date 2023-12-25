# 五子棋_C语言小组作业_ZJUT
`浙江工业大学` `信息工程学院` `电气类2304` `完蛋，我被代码包围了!小组`
## 应用介绍
### 简介
该应用在C语言环境下编写的一款集双人对战和人机对战的益智类五子棋游戏，具有基础的算法来实现电脑与玩家的对战，玩家每次进行游戏过程中都会有分数记录并可在游戏结束后查看。
### 软件特点
1. 界面简洁：该软件采用简洁明了的界面设计，易于上手。您只需通过简单的点击或滑动操作，即可轻松实现落子。
2. 智能对弈：内置基础的人工智能算法，可与您进行智能对弈。系统会自动识别并优化棋局，让您体验到与真实对手对弈的乐趣。
3. 多种模式：支持单人、双人模式。您可以随时邀请好友，增进友谊，锻炼策略思维。
4. 记录：软件会记录您的对战，您可以通过分析数据，不断提升自己的五子棋水平。
### 使用指南
1. 安装与启动：安装该软件后，打开应用即可开始五子棋之旅。
2. 选择对战模式：在主界面选择人机、双人模式，开始您的五子棋之旅。
3. 操作说明：通过点击屏幕上的棋盘，选择落子位置。当您完成落子后，系统会自动识别并更新棋局。
4. 查看与分析：在结束一局对战后，您可以查看对战记录、分数等数据，以便分析自己的表现并不断进步。
### 应用模式
双人模式和人机模式
### 游戏规则
#### 双人模式
1. 与好友自行协商先后手。
2. 每一轮每人持一子，分先后落子，每次落子得分1分，获胜得30分。
3. 双方中有一方在任一直线（或直对角线上）完成5颗连续落子则为获胜。
4. 第一轮由绿子方占先手，之后依次落子，中途不可悔棋，直到一方胜出。
#### 人机模式
1. 己方为先手方，电脑为后手方。
2. 每轮一次落子机会，落子后等待电脑玩家完成落子后方可继续落子，每次落子得1分，获胜得30分。
3. 双方中有一方在任一直线（或直对角线上）完成5颗连续落子则为获胜。
4. 第一轮由绿子方占先手，之后依次落子，中途不可悔棋，直到一方胜出。
## 代码逻辑分析
### 页面框架
为了有更好的图形化界面，我们采用了EasyX图形库，可以绘制图形。
#### 导入库函数
```C
/*导入包*/
#include <graphics.h>
#include <stdio.h>
#include <io.h>
#include <stdlib.h>
#include <string.h>
#include <direct.h>
#include <time.h>
#include <malloc.h>
#include <sys/types.h>
#include <dirent.h>
```
#### 宏定义
```C
/*宏定义*/
#define windowWidth 460 //初始窗口宽度 460
#define windowHeight 680 //初始窗口高度 680
#define marginTop 50 //页面元素距离顶部距离
#define GREEN10 RGB(114,162,140) //绿色1
#define GREEN20 RGB(236,246,238) //绿色2
#define GREEN30 RGB(225,238,228) //绿色3
#define GREEN40 RGB(33,106,76) //绿色4
#define bkColor RGB(248,253,247) //背景颜色
#define RANKPAGE 3 //游戏得分记录页面
#define ABOUTPAGE 2 //游戏关于页面
#define PLAYPAGE 1 //游戏下棋页面
#define INITPAGE 0 //游戏初始页面
#define REVIEWPAGE 8 //棋局再现页面
#define ERRORPAGE 4 //错误-页面
#define ERRORFIX 5 //错误-修复
#define ERRORCREATFOLDER 6 //错误-创建文件夹
#define ERRORPUTRECORD 7 //错误-写入游戏
#define ERROROPENFOLDER 9 //错误-打开文件夹
#define COMPUTERGAME 0 //人机对战模式
#define DOUBLEGAME 1 //双人对战模式
#define ME 0 //己方
#define YOU 1 //对方
#define EOL 10 //End of List
#define HOL 11 //Head of List设置窗口句柄
/*设置窗口句柄*/
int changeHwnd() {
    HWND hnd = GetHWnd();
    SetWindowText(hnd, "五子棋_C语言小组作业");
    return 1;
}
```
#### 初始化页面绘制
```C
/*初始化界面*/
int windowInit(int x) {
    switch (x) {
    case RANKPAGE: {//记录界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口（460+20，640，禁止窗口最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色
        cleardevice();//清屏
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(38, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "游戏记录");//输出文字（X位置，Y位置，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "记录您最近10次游戏的得分");//输出文字（X，Y，文字）
        break;
    }
    case ABOUTPAGE: {//关于界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);  //创建窗口（宽度，高度，禁止最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（背景色RGB）
        cleardevice();//清屏        
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(38, 0, "微软雅黑");//设置文字样式（高度，间隔，字体） 
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "关于");//输出文字（X，Y，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "浙江工业大学_程序设计基础C语言作业");//输出文字（X，Y，文字）
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 95, "小组成员");//输出文字（X，Y，文字）
        settextstyle(26, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）          
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 130, "白展硕_302023510113_应用功能/算法/文档");//输出文字（X，Y，文字）       
        outtextxy(40, 155, "章方舟_302023510106");//输出文字（X，Y，文字）         
        outtextxy(40, 180, "杨   晨_302023510125_文档");//输出文字（X，Y，文字）         
        outtextxy(40, 205, "龙   勇_302023510119_注释");//输出文字（X，Y，文字）         
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 240, "应用介绍");//输出文字（X，Y，文字）
        settextstyle(26, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 275, "该应用采用C语言编写，其有两种游戏模式，");//输出文字（X，Y，文字）         
        outtextxy(40, 300, "双人游戏可以两个真人分别落子下棋；");//输出文字（X，Y，文字）         
        outtextxy(40, 325, "人机对战可以让你和机器人下棋对战。");//输出文字（X，Y，文字）       
        settextcolor(BLUE);
        outtextxy(40, 350, "浏览更多...");//输出文字（X，Y，文字       
        setfillcolor(GREEN30);//设置填充颜色（RGB）
        setlinecolor(GREEN30);//设置线条颜色（RGB）
        fillroundrect(170, 450, 290, 490, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）        
        settextcolor(GREEN40);//设置文字颜色（RGB）
        outtextxy(208, 454, "返回");;//输出文字（X，Y，文字）         
        setlinecolor(GREEN10);//设置线条颜色（RGB）
        while (1) {
            ////printf("WW");
            if (MouseHit()) {//点击鼠标操作
                MOUSEMSG msg = GetMouseMsg();//定义
                switch (msg.uMsg) {//结构体
                case WM_LBUTTONDOWN://点击左键
                    //printf("%d,%d", msg.x, msg.y);
                    getMouseClickBtn(msg.x, msg.y, ABOUTPAGE);//获取鼠标位置
                }
            }
        }
        break;
    }
    case PLAYPAGE: {//对战界面左上的“五子棋”
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口  
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（RGB）                                   
        cleardevice();//清屏                                       
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(25, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT); //设置背景模式（透明）                  
        outtextxy(20, 10, "五子棋");//输出文字       
        drawChessBoard();//边框
        break;
    };
    case INITPAGE: {//进入APP初始界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//设置背景颜色（RGB） 
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（RGB）                                   
        cleardevice();//清屏                                       
        settextcolor(GREEN40);//设置文字颜色（RGB                      
        settextstyle(40, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(185, 170, "五子棋");//输出文字         ;
        setfillcolor(GREEN30);//设置填充颜色
        setlinecolor(GREEN30);//设置线条颜色
        fillroundrect(170, 330, 290, 370, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(188, 334, "双人游戏");//输出文字         
        fillroundrect(170, 390, 290, 430, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式           
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(188, 394, "人机对战");//输出文字          
        fillroundrect(170, 450, 290, 490, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(210, 454, "关于");//输出文字 
        fillroundrect(170, 510, 290, 550, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(188, 514, "得分记录");//输出文字 
        fillroundrect(170, 570, 290, 610, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(188, 574, "棋局再现");//输出文字 
        setlinecolor(GREEN10);//设置线条颜色
        break;
    };
    case REVIEWPAGE: {
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口（460+20，640，禁止窗口最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色
        cleardevice();//清屏
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(38, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）

    }
    }
    return 1;
}
```
### 人机模式实现
点击进入人机对战后，就会进入newComputerGame()函数，再执行windowInit(int x)函数来初始化界面。后执行drawChessBoard()函数来绘制棋盘，见绘制棋盘。
```C
int newComputerGame() {//人机对战
    windowInit(PLAYPAGE);//初始界面
    dataInit();
    creatPlayRecord(COMPUTERGAME);
    while (1) {
        if (MouseHit()) {
            MOUSEMSG msg = GetMouseMsg();//获取鼠标位置
            switch (msg.uMsg) {
            case WM_LBUTTONDOWN://左键
                getMouseClick(msg.x, msg.y, 1);//鼠标在哪个页面点的什么位置
                if (isFiveLink(mouseClickX, mouseClickY)) {//判断玩家是否五个连成一起
                    putPlayRecord( 0, 0, -1);
                    score += 30;   //加30分
                    drawEndPage(1);//本局玩家结束页面
                    clickEndBtn();//点击结束按钮
                }
                if (canContinue) {
                    canContinue = 0;
                    drawComputerChess();//电脑下棋位置
                    if (isYouFiveLink(computerX, computerY)) {//电脑是否赢
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(0);//本局电脑结束页面
                        clickEndBtn();//点击结束按钮
                    }
                }
                break;
            case WM_RBUTTONDOWN://右键则不操作
                break;
            }
        }
    }
    return 1;
}
```
#### 绘制棋盘
```C
/*制作棋盘基本框架*/
int drawChessBoard() {
    //边框
    setlinestyle(PS_SOLID, 3);//设置线条样式
    setlinecolor(GREEN40);//设置线条颜色
    line(10, 10 + marginTop, 10, 450 + marginTop);
    line(10, 10 + marginTop, windowWidth - 10, 10 + marginTop);
    line(windowWidth - 10, 10 + marginTop, windowWidth - 10, 450 + marginTop);
    line(10, 450 + marginTop, windowWidth - 10, 450 + marginTop);
    //10*10表格
    setlinestyle(PS_SOLID, 2);
    setlinecolor(GREEN10);
    for (int i = 0; i < 10; i++) {
        line(10, 50 + 40 * i + marginTop, windowWidth - 10, 50 + 40 * i + marginTop);
    }
    for (int i = 0; i < 10; i++) {
        line(50 + 40 * i, 10 + marginTop, 50 + 40 * i, 450 + marginTop);
    }
    //文字标号
    settextcolor(GREEN40);
    settextstyle(20, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    outtextxy(45, marginTop - 10, "1        2        3        4         5        6        7       8         9       10");
    for (int i = 0; i < 10; i++) {
        outtextxy(460, marginTop + 40 + 40 * i, 'a' + i);
    }
    return 1;
}
```
#### 玩家下棋
首先获取玩家鼠标的绝对位置坐标，然后使用getMouseClick(int x, int y, int z)将绝对坐标转化为对应棋盘坐标，并且执行isChessCanDown(mouseClickX, mouseClickY, z)来判断该位置能否下棋，如果能下棋，则执行drawMyChess(x, y)函数为玩家在相应位置落子，如果不能下棋，则函数会返回对应值。
```C
/*玩家下棋*/
int drawMyChess(int x, int y) {//下我的棋子
    setfillcolor(GREEN40);
    fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
    state[x][y] = 1;
    score++;
    changeWeight(x, y);//改变权重
    canContinue = 1;
    putPlayRecord( x, y, ME);
    return 1;
}
```
#### 电脑下棋
```C
/*电脑下棋*/
int drawComputerChess() {
    //初始化下棋坐标
    int x = 0;
    int y = 0;
    computerX = 0;
    computerY = 0;
    //初始化是否找到下棋位置
    int isFind = 0;
    int max = 0;
    for (int i = 0; i < 10; i++) {
        for (int k = 0; k < 10; k++) {
            if (weight[i][k] > max&& state[i][k] == 0) {
                max = weight[i][k];
            }
        }
    }
    for (int i = 0; i < 10; i++) {
        for (int k = 9; k >= 0; k--) {
            if (weight[i][k] == max && state[i][k] == 0) {
                x = i;
                y = k;
                isFind = 1;
                i + 10;

            }
        }
    }
    while (isFind != 1) {
        srand((unsigned int)time(NULL));
        int ret1 = rand() % 10;
        int ret2 = rand() % 10;
        x = ret1;
        y = ret2;
        if (state[x][y] == 0) {
            isFind = 1;
        }
    }
    if (state[x][y] == 0) {
        setfillcolor(bkColor);
        fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
        computerX = x;
        computerY = y;
        state[x][y] = 2;
        changeWeight(x, y);
        canContinue = 0;
        putPlayRecord( x, y, YOU);
        return 1;
    }
    else {
        //printf("ERROR\n");
    }

}
```
#### 输赢判断
人机对战中，每一轮下棋都会使用isFiveLink(mouseClickX, mouseClickY)函数判断玩家是否五子连线，若玩家获胜，则分数+30，触发drawEndPage(1)函数进入玩家结束界面，并使用clickEndBtn()函数判断用户是否点击了结束按钮。
```C
/*5连_玩家能否赢的算法*/
int isFiveLink(int x, int y) {//是否玩家五个连成一条线（判断是否游戏结束算法）drawFiveLinkLine即画线
    //oooo_（最右边差一个）
    //printf("%d %d\n",x,y);
    if (state[x - 1][y] == 1 && state[x - 2][y] == 1 && state[x - 3][y] == 1 && state[x - 4][y] == 1) {
        drawFiveLinkLine(x, y, x - 4, y);
        return 1;
    }
    //ooo_o（中间差一个）
    else if (state[x + 1][y] == 1 && state[x - 1][y] == 1 && state[x - 2][y] == 1 && state[x - 3][y] == 1) {
        drawFiveLinkLine(x + 1, y, x - 3, y);
        return 1;
    }
    //oo_oo（中间差一个）
    else if (state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x - 1][y] == 1 && state[x - 2][y] == 1) {
        drawFiveLinkLine(x + 1, y, x - 2, y);
        return 1;
    }
    //o_ooo（中间差一个）
    else if (state[x - 1][y] == 1 && state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x + 3][y] == 1) {
        drawFiveLinkLine(x - 1, y, x + 3, y);
        return 1;
    }
    //_oooo（最左边差一个）
    else if (state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x + 3][y] == 1 && state[x + 4][y] == 1) {
        drawFiveLinkLine(x, y, x + 4, y);
        return 1;
    }
    //oooo__y（竖的方向最下面差一个）
    else if (state[x][y - 1] == 1 && state[x][y - 2] == 1 && state[x][y - 3] == 1 && state[x][y - 4] == 1) {
        drawFiveLinkLine(x, y, x, y - 4);
        return 1;
    }
    //ooo_o_y（竖的方向中间差一个）
    else if (state[x][y + 1] == 1 && state[x][y - 1] == 1 && state[x][y - 2] == 1 && state[x][y - 3] == 1) {
        drawFiveLinkLine(x, y + 1, x, y - 3);
        return 1;
    }
    //oo_oo_y（竖的方向中间差一个）
    else if (state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y - 1] == 1 && state[x][y - 2] == 1) {
        drawFiveLinkLine(x, y + 2, x, y - 2);
        return 1;
    }
    //o_ooo_（竖的方向中间差一个）
    else if (state[x][y - 1] == 1 && state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y + 3] == 1) {
        drawFiveLinkLine(x, y - 1, x, y + 3);
        return 1;
    }
    //_oooo_y（竖的方向最上面差一个）
    else if (state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y + 3] == 1 && state[x][y + 4] == 1) {
        drawFiveLinkLine(x, y, x, y + 4);
        return 1;
    }
    //oooo__left|top_to_right|bottom（左上到右下差一个）
    else if (state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1 && state[x - 3][y - 3] == 1 && state[x - 4][y - 4] == 1) {
        drawFiveLinkLine(x, y, x - 4, y - 4);
        return 1;
    }
    //ooo_o_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x + 1][y + 1] == 1 && state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1 && state[x - 3][y - 3] == 1) {
        drawFiveLinkLine(x + 1, y + 1, x - 3, y - 3);
        return 1;
    }
    //oo_oo_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1) {
        drawFiveLinkLine(x + 2, y + 2, x - 2, y - 2);
        return 1;
    }
    //o_ooo_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x - 1][y - 1] == 1 && state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x + 3][y + 3] == 1) {
        return 1;
    }
    //_oooo_left|top_to_right|bottom（左上到右下差一个）
    else if (state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x + 3][y + 3] == 1 && state[x + 4][y + 4] == 1) {
        drawFiveLinkLine(x, y, x + 4, y + 4);
        return 1;
    }
    //oooo__left|bottom_to_right|top（左下到右上差一个）
    else if (state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1 && state[x - 3][y + 3] == 1 && state[x - 4][y + 4] == 1) {
        drawFiveLinkLine(x, y, x - 4, y + 4);
        return 1;
    }
    //ooo_o_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x + 1][y - 1] == 1 && state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1 && state[x - 3][y + 3] == 1) {
        drawFiveLinkLine(x + 1, y - 1, x - 3, y + 3);
        return 1;
    }
    //oo_oo_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1) {
        drawFiveLinkLine(x + 2, y - 2, x - 2, y + 2);
        return 1;
    }
    //o_ooo_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x - 1][y + 1] == 1 && state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x + 3][y - 3] == 1) {
        drawFiveLinkLine(x + 3, y - 3, x - 1, y + 1);
        return 1;
    }
    //_oooo_left|bottom_to_right|top（左下到右上差一个）
    else if (state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x + 3][y - 3] == 1 && state[x + 4][y - 4] == 1) {
        drawFiveLinkLine(x + 4, y - 4, x, y);
        return 1;
    }

    return 0;
}
/*5连_电脑/对方能否赢的算法*/
int isYouFiveLink(int x, int y) {//以下为判断电脑是否胜的算法
    //oooo_
    //printf("===%d %d\n", x, y);
    if (state[x - 1][y] == 2 && state[x - 2][y] == 2 && state[x - 3][y] == 2 && state[x - 4][y] == 2) {
        drawFiveLinkLine(x, y, x - 4, y);
        return 1;
    }
    //ooo_o
    else if (state[x + 1][y] == 2 && state[x - 1][y] == 2 && state[x - 2][y] == 2 && state[x - 3][y] == 2) {
        drawFiveLinkLine(x + 1, y, x - 3, y);
        return 1;
    }
    //oo_oo
    else if (state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x - 1][y] == 2 && state[x - 2][y] == 2) {
        drawFiveLinkLine(x + 1, y, x - 2, y);
        return 1;
    }
    //o_ooo
    else if (state[x - 1][y] == 2 && state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x + 3][y] == 2) {
        drawFiveLinkLine(x - 1, y, x + 3, y);
        return 1;
    }
    //_oooo
    else if (state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x + 3][y] == 2 && state[x + 4][y] == 2) {
        drawFiveLinkLine(x, y, x + 4, y);
        return 1;
    }
    //oooo__y
    else if (state[x][y - 1] == 2 && state[x][y - 2] == 2 && state[x][y - 3] == 2 && state[x][y - 4] == 2) {
        drawFiveLinkLine(x, y, x, y - 4);
        return 1;
    }
    //ooo_o_y
    else if (state[x][y + 1] == 2 && state[x][y - 1] == 2 && state[x][y - 2] == 2 && state[x][y - 3] == 2) {
        drawFiveLinkLine(x, y + 1, x, y - 3);
        return 1;
    }
    //oo_oo_y
    else if (state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y - 1] == 2 && state[x][y - 2] == 2) {
        drawFiveLinkLine(x, y + 2, x, y - 2);
        return 1;
    }
    //o_ooo_y
    else if (state[x][y - 1] == 2 && state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y + 3] == 2) {
        drawFiveLinkLine(x, y - 1, x, y + 3);
        return 1;
    }
    //_oooo_y
    else if (state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y + 3] == 2 && state[x][y + 4] == 2) {
        drawFiveLinkLine(x, y, x, y + 4);
        return 1;
    }
    //oooo__left|top_to_right|bottom
    else if (state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2 && state[x - 3][y - 3] == 2 && state[x - 4][y - 4] == 2) {
        drawFiveLinkLine(x, y, x - 4, y - 4);
        return 1;
    }
    //ooo_o_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2 && state[x - 3][y - 3] == 2) {
        drawFiveLinkLine(x + 1, y + 1, x - 3, y - 3);
        return 1;
    }
    //oo_oo_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2) {
        drawFiveLinkLine(x + 2, y + 2, x - 2, y - 2);
        return 1;
    }
    //o_ooo_left|top_to_right|bottom
    else if (state[x - 1][y - 1] == 2 && state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x + 3][y + 3] == 2) {
        return 1;
    }
    //_oooo_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x + 3][y + 3] == 2 && state[x + 4][y + 4] == 2) {
        drawFiveLinkLine(x, y, x + 4, y + 4);
        return 1;
    }
    //oooo__left|bottom_to_right|top
    else if (state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2 && state[x - 3][y + 3] == 2 && state[x - 4][y + 4] == 2) {
        drawFiveLinkLine(x, y, x - 4, y + 4);
        return 1;
    }
    //ooo_o_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2 && state[x - 3][y + 3] == 2) {
        drawFiveLinkLine(x + 1, y - 1, x - 3, y + 3);
        return 1;
    }
    //oo_oo_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2) {
        drawFiveLinkLine(x + 2, y - 2, x - 2, y + 2);
        return 1;
    }
    //o_ooo_left|bottom_to_right|top
    else if (state[x - 1][y + 1] == 2 && state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x + 3][y - 3] == 2) {
        drawFiveLinkLine(x + 3, y - 3, x - 1, y + 1);
        return 1;
    }
    //_oooo_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x + 3][y - 3] == 2 && state[x + 4][y - 4] == 2) {
        drawFiveLinkLine(x + 4, y - 4, x, y);
        return 1;
    }
    return 0;
}
```
#### 结束界面
drawEndPage(int x)函数被用于绘制结束界面，当传入参数为1时，为玩家获胜结束界面，为0时为电脑获胜结束界面，结束后使用putRankFile()函数提交得分。
```C
/*页面结算*/
int drawEndPage(int x) {//本局结束页面
    setfillcolor(GREEN30);//设置填充颜色
    setlinecolor(GREEN30);//设置线条颜色
    fillroundrect(170, 530, 290, 570, 40, 40);//有填充的圆角
    if (x) {
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//文字样式
        setbkmode(TRANSPARENT);//背景模式
        outtextxy(195, 534, "你赢了！");//输出文字
    }
    else {
        settextcolor(GREEN40);//文字颜色
        settextstyle(30, 0, "微软雅黑");//文字样式
        setbkmode(TRANSPARENT);//背景模式
        outtextxy(195, 534, "对方胜！");//输出文字
    }
    putRankFile();//提交得分
    //printf("%d\n", score);
    /*绘制确定按钮*/
    settextcolor(GREEN40);
    settextstyle(22, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    outtextxy(214, 575, "确定");
    return 1;
}
```
### 双人模式实现
点击进入双人模式后，就会进入newDoubleGame()函数，再执行windowInit(int x)函数来初始化界面。后执行drawChessBoard()函数来绘制棋盘，见绘制棋盘
```C
/*双人对战*/
int newDoubleGame() {//双人对战
    windowInit(PLAYPAGE);//初始化界面
    dataInit();
    creatPlayRecord(DOUBLEGAME);
    while (1) {
        if (canContinue == 0) {
            if (MouseHit()) {
                MOUSEMSG msg = GetMouseMsg();
                switch (msg.uMsg) {
                case WM_LBUTTONDOWN://左键
                    getMouseClick(msg.x, msg.y, 1);//鼠标点的位置

                    if (isFiveLink(mouseClickX, mouseClickY)) {//第一个玩家是否五个一条线
                        score += 30;//加分
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(1);//画
                        clickEndBtn();//点击结束按钮
                    }


                    break;
                case WM_RBUTTONDOWN://右键
                    break;
                }
            }
        }
        if (canContinue) { //轮到对方操作
            if (MouseHit()) {
                MOUSEMSG msg = GetMouseMsg();
                switch (msg.uMsg) {
                case WM_LBUTTONDOWN:
                    getMouseClick(msg.x, msg.y, 0);//对方鼠标位置
                    if (isYouFiveLink(doubleX, doubleY)) {//对方能否赢
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(0);//画
                        clickEndBtn();//点击结束按钮
                    }
                    break;
                case WM_RBUTTONDOWN:
                    break;
                }
            }
        }

    }
    return 1;
}
```
#### 绘制棋盘
##### 先手下棋
首先获取玩家鼠标的绝对位置坐标，然后使用getMouseClick(int x, int y, int z)将绝对坐标转化为对应棋盘坐标，并且执行isChessCanDown(mouseClickX, mouseClickY, z)来判断该位置能否下棋，如果能下棋，则执行drawMyChess(x, y)函数为玩家在相应位置落子，如果不能下棋，则函数会返回对应值。
```C
if (MouseHit()) {
    MOUSEMSG msg = GetMouseMsg();
    switch (msg.uMsg) {
    case WM_LBUTTONDOWN://左键
        getMouseClick(msg.x, msg.y, 1);//鼠标点的位置
    }
}
int getMouseClick(int x, int y, int z) {//获取鼠标点击位置
    for (int i = 0; i < 10; i++) {
        for (int k = 0; k < 10; k++) {
            if (x > (50 + 40 * k - 15) && x<(50 + 40 * k + 15) && y>(marginTop + 50 + 40 * i - 15) && y < (marginTop + 50 + 40 * i + 15)) {
                mouseClickX = k;
                mouseClickY = i;
                isChessCanDown(mouseClickX, mouseClickY, z);  //棋子是否可以下
                return 1;
            }
        }
    }
}
int isChessCanDown(int x, int y, int z) {//该位置是否可以下棋
    switch (state[x][y]) {
    case 0://如果没有棋子，则绘制棋子并返回0
        switch (z) {
        case 1:drawMyChess(x, y); break;//下玩家的棋子
        case 0:drawYourChess(x, y);//下对方的棋子
        }
        return 0;
        break;
    case 1://如果有己方棋子，则返回1
        switch (z) {
        case 1:canContinue = 0; break;//对方不能下棋
        case 0:canContinue = 1; break;//对方可以下棋
        }
        return 1;
    case 2://如果有对方棋子，则返回2
        switch (z) {
        case 1:canContinue = 0; break;
        case 0:canContinue = 1; break;
        }
        return 2;
    default://出错，返回-1
        switch (z) {
        case 1:canContinue = 0; break;
        case 0:canContinue = 1; break;
        }
        return -1;
    }
}
int drawMyChess(int x, int y) {//下我的棋子
    setfillcolor(GREEN40);
    fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
    state[x][y] = 1;//该位置已有棋
    score++;
    changeWeight(x, y);//改变权重
    canContinue = 1;
    return 1;
}
```
##### 后手下棋
更多详情可见先手下棋中getMouseClick(int x, int y, int z)、isChessCanDown(mouseClickX, mouseClickY, z)、drawMyChess(x, y)函数。
```C
if (MouseHit()) {
    MOUSEMSG msg = GetMouseMsg();
    switch (msg.uMsg) {
    case WM_LBUTTONDOWN:
        getMouseClick(msg.x, msg.y, 0);//对方鼠标位置
    }
}
```
#### 输赢判断
通过两次if语句经过isFiveLink()函数判断双方落子后是否有一方胜出，并将参数0或1传给drawEndPage()函数，绘制结束界面，再通过clickEndBtn()函数判断是否点击了结束按钮。关于isFiveLink()、drawEndPage()、clickEndBtn()函数的更多详情可见输赢判断和结束界面。
```C
if (isFiveLink(mouseClickX, mouseClickY)) {//第一个玩家是否五个一条线
    score += 30;//加分
    drawEndPage(1);//画线
    clickEndBtn();//点击结束按钮
}
if (isYouFiveLink(doubleX, doubleY)) {//对方能否赢
    drawEndPage(0);//画线
    clickEndBtn();//点击结束按钮
}
```
### 记分系统实现
#### 得分规则
每次落子得1分，获胜再得30分。
#### 文件写入得分
读取res/Rank.txt文件，不存在则尝试创建，不成功则弹窗报错，读取文件原有内容，第一行为数据个数，第一行+1，在数据末尾添加项，再重新写入文件中。
```C
int putRankFile() {
    /*先读*/
    FILE* fp = fopen("res/Rank.txt", "r");
    /*判空*/
    if (fp == NULL) {
        printf("请使用管理员模式打开程序");
        return 0;
    }
    /*整合数据*/
    char content[100][100] = { 0 };
    char line[10] = { 0 };
    fgets(line, 10, fp);
    int newLine = atoi(line);
    for (int i = 0; i < newLine; i++) {
        fgets(content[i], 100, fp);
    }
    itoa(newLine + 1, line, 10);
    fclose(fp);
    /*再写*/
    FILE* fp1 = fopen("res/Rank.txt", "w");
    itoa(newLine + 1, line, 10);
    char str[10] = { 0 };
    if (newLine == 0) {

    }
    else {
        strcat(line, "\n");
    }
    fputs(line, fp1);
    for (int i = 0; i < newLine; i++) {
        fputs(content[i], fp1);
    }
    itoa(score, str, 10);
    fputs("\n", fp1);
    fputs(str, fp1);
    fclose(fp1);
    return 1;
}
```
#### 文件读取得分
读取res/Rank.txt文件，不存在则尝试创建，不成功则弹窗报错，读取文件原有内容，第一行为数据个数，绘制窗口框架，如果文件条数大于10，则循环读取10条数据并显示，超出部分不读取，如果小于10条则读取对应数目并显示。
```C
/*获取得分记录*/
int getRankFile() {
    if (!access("res", 0)) {
    }

    else {
        creatMessage(ERRORFIX);
        drawRankPage(1);
        return 0;
    }

    if (!access("res/Rank.txt", 0)) {
    }

    else {
        FILE* fp2 = fopen("res/Rank.txt", "w+");
        fputs("0", fp2);
        fclose(fp2);
    }

    FILE* fp = fopen("res/Rank.txt", "r");
    /*判空*/
    if (fp == NULL) {
        creatMessage(ERRORPAGE);
        printf("请使用管理员模式打开程序\n");
        return 0;
    }

    /*读取_传参_绘制*/
    char a[10];
    fgets(a, 10, fp);
    int b = atoi(a);
    //printf("%d\n", b);
    char content[100] = { 0 };
    if (b == 0) {
        settextcolor(GREEN40);
        settextstyle(30, 0, "微软雅黑");
        setbkmode(TRANSPARENT);
        outtextxy(190, 170, "暂无记录");
    }
    else if (b > 10) {
        for (int i = 0; i < 10; i++) {
            fgets(a, 10, fp);
            drawRankItems(i + 1, a);
        }
    }
    else {
        for (int i = 0; i < b; i++) {
            fgets(a, 10, fp);
            drawRankItems(i + 1, a);
        }
    }
    drawRankPage(1);

    /*关闭文件*/
    fclose(fp);
    return 1;
}
```
#### 清空得分记录
读取res/Rank.txt文件，不存在则尝试创建，不成功则弹窗报错，清空写入0。
```C
int clearRankFile() {
    FILE* fp1 = fopen("res/Rank.txt", "w");
    fputs("0", fp1);
    fclose(fp1);
    initPage();//再次打开界面
    closegraph();//关闭界面
    return 1;
}
```
### 棋局存取系统实现
#### 文件写入记录
以人机对战为例，当进入人机对战模式后，就会触发creatPlayRecord()函数创建一个游戏记录文件，并且再每一次下棋后触发putPlayRecord()函数来追加写入一行下棋记录。
其中下棋记录文件格式如下：
```
FilePath
Mode
Who:(x,y)
Who:(x,y)
Who:(x,y)
...
-1:(0,0)
```
```C
/*NEW:创建游戏记录文件*/
char* creatPlayRecord(int mode) {
    int res;
    if (!access("record", 0)) {
    }
    else {
        res = mkdir("record");// 返回 0 表示创建成功，-1 表示失败
        if (res == 0) {
        }
        else {
            creatMessage(ERRORCREATFOLDER);
        }
    }
    time_t timep;
    struct tm* p;
    //static char kpath[200] = {};
    char content[200] = {};
    time(&timep);
    p = gmtime(&timep);
    sprintf(kpath, "record/%d%02d%02d_%02d%02d%02d.txt\0", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
    switch (mode) {
    case COMPUTERGAME: {
        sprintf(content, "record/%d%02d%02d_%02d%02d%02d.txt\n人机模式\n", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
        break;
    }
    case DOUBLEGAME: {
        sprintf(content, "record/%d%02d%02d_%02d%02d%02d.txt\n双人模式\n", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
        break;

    }
    }
    FILE* fp = fopen(kpath, "w+");
    fputs(content, fp);
    fclose(fp);
    return kpath;
}
/*NEW:更新游戏记录文件*/
int putPlayRecord(int x, int y, int who) {
    //printf("put:%s\n",kpath);
    if (!access(kpath, 0)) {
    }
    else {
       // printf("=============\n");
        creatMessage(ERRORPUTRECORD);
    }
    FILE* fp = fopen(kpath, "a");
    fprintf(fp,"%d:(%d,%d)\n",who,x,y);
    fclose(fp);
    return 1;
}
```
#### 文件读取记录
通过drawRecordFileList()得到用户希望再现的文件，传入文件路径，尝试打开文件读取，不成功则弹窗报错。循环读取文件数据，并在棋盘中绘制，直到 End of File 。
```C
/*NEW:恢复棋盘*/
int recoveryByPlayRecord(const char kpath[]) {
    dataInit();
    //printf("%s", kpath);
    char fpath[100];
    strcpy(fpath, kpath);
    //printf("%s", fpath);
    if (!access(fpath, 0)) {
    }
    else {
        creatMessage(ERRORPUTRECORD);
    }
    FILE* fp = fopen(fpath, "r");
    char rmode[50] = {};
    char ch[100];
    //printf("\n");
    fgets(ch, 100, fp);
    //printf("%s",ch);
    fgets(ch, 100, fp);
    //printf("%s", ch);
    windowInit(PLAYPAGE);//初始化界面
    while (1) {
        fscanf(fp, "%d:(%d,%d)", &rwho, &rx, &ry);
        if (rwho == ME) {
            setfillcolor(GREEN40);
            fillcircle(50 + 40 * rx, 50 + marginTop + 40 * ry, 10);
            state[rx][ry] = 1;
            isFiveLink(rx, ry);
        }
        else if (rwho == YOU) {
            setfillcolor(bkColor);
            fillcircle(50 + 40 * rx, 50 + marginTop + 40 * ry, 10);
            state[rx][ry] = 2;
            isYouFiveLink(rx, ry);
        }
        else {
            break;
        }
        Sleep(300);
    }
    setfillcolor(GREEN30);//设置填充颜色（RGB）
    setlinecolor(GREEN30);//设置线条颜色（RGB）
    fillroundrect(170, 550, 290, 590, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
    settextcolor(GREEN40);//设置文字颜色（RGB）                      
    settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
    setbkmode(TRANSPARENT);//设置背景模式（透明）                   
    outtextxy(208, 554, "返回");;//输出文字（X，Y，文字）         
    setlinecolor(GREEN10);//设置线条颜色（RGB）
    while (1) {
        if (MouseHit()) {//点击鼠标操作
            MOUSEMSG msg = GetMouseMsg();//定义
            switch (msg.uMsg) {//结构体
            case WM_LBUTTONDOWN://点击左键
                getMouseClickBtn(msg.x, msg.y, REVIEWPAGE);//获取鼠标位置
            }
        }
    }
    fclose(fp);
    return 1;
}
```
#### 棋局再现列表页面
读取record文件夹下所有文件，并存放到数组中，根据文件数量/15求得页面数量（一页只能显示15条数据），利用page和item两个变量确定用户点击项目，并将对应文件路径传给函数recoveryByPlayRecord(const char kpath[])来恢复棋局。
```C
/*NEW:棋局再现_文件列表*/
int drawRecordFileList() {
    windowInit(REVIEWPAGE);
    char fileNameList[200][70] = {};
    int page = 0;
    int item = 0;
    int res;
    if (!access("record", 0)) {
    }
    else {
        res = mkdir("record");// 返回 0 表示创建成功，-1 表示失败
        if (res == 0) {
        }
        else {
            creatMessage(ERRORCREATFOLDER);
        }
    }
    DIR* dir = opendir("./record");
    //成功：返回指向该目录的结构体目录
    //失败：返回NULL
    if (dir == NULL)
    {
        //printf("打开失败！\n");
        creatMessage(ERROROPENFOLDER);
    }
    //定义一个目录结构体题指针
    struct dirent* dirp;
    int i = 0;
    while (1)
    {
        dirp = readdir(dir);
        //readdir打开目录，返回值为一个结构体
        if (dirp == NULL)
        {
            break;
        }
        //dirp->d_type 是这个指针指向文件的类型
        //DT_DIR  目录
        //DT_REG  文件
        if (dirp->d_type == DT_DIR)
        {
        }
        else if (dirp->d_type == DT_REG)
        {
            strcpy(fileNameList[i++], dirp->d_name);
        }
        else
        {
            break;
        }

    }
    //关闭目录
    closedir(dir);
    //printf("\n\n");
    drawReviewPageBtn();
    int fileNameListLen = 0;
    for (int k = 0; 0 != strcmp(fileNameList[k], ""); k++) {
        //printf("%s\n",fileNameList[k]);

        fileNameListLen = k;
    }
    char str3[100] = "record/";
    while (1) {

        for (int j = 15 * page; j < 15 * (page + 1) && 0 != strcmp(fileNameList[j], ""); j++) {
            drawRecordFileListItem(j, page, fileNameList[j]);
        }
        if (MouseHit()) {//点击鼠标操作
            MOUSEMSG msg = GetMouseMsg();//定义
            switch (msg.uMsg) {//结构体
            case WM_LBUTTONDOWN://点击左键
                switch (getMouseClickBtn(msg.x, msg.y, REVIEWPAGE)) {
                case 0:item = 0; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 1:item = 1; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 2:item = 2; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 3:item = 3; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 4:item = 4; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 5:item = 5; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 6:item = 6; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 7:item = 7; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 8:item = 8; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 9:item = 9; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 10:item = 10; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 11:item = 11; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 12:item = 12; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 13:item = 13; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 14:item = 14; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case -2: {
                    if (page + 1 > 1. * fileNameListLen / 15) {
                        //printf("ENDOFLIST\n");
                        creatMessage(EOL);
                    }
                    else {
                        page++;
                        cleardevice();
                        setbkcolor(bkColor);//设置背景颜色
                        cleardevice();//清屏
                        settextcolor(GREEN40);//设置文字颜色
                        settextstyle(38, 0, "微软雅黑");//设置文字样式
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
                        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
                        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）
                        drawReviewPageBtn();
                    }
                    break;
                }
                case -3: {

                    if (page - 1 < 0) {
                        creatMessage(HOL);
                        //printf("HEADOFLIST\n");
                    }
                    else {
                        page--;
                        cleardevice();
                        setbkcolor(bkColor);//设置背景颜色
                        cleardevice();//清屏
                        settextcolor(GREEN40);//设置文字颜色
                        settextstyle(38, 0, "微软雅黑");//设置文字样式
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
                        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
                        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）
                        drawReviewPageBtn();
                    }
                    break;
                }
                }

            }
        }
    }
    return 1;
}
/*NEW:绘制列表项目*/
int drawRecordFileListItem(int i, int page, char fileName[]) {
    settextstyle(26, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    char str[20];
    char str2[20];
    itoa(i + 1, str, 10);

    itoa(page + 1, str2, 10);
    strcat(str2, "-");
    outtextxy(40, 120 + 25 * (i - 15 * page), strcat(str2, str));//绘制序号
    outtextxy(120, 120 + 25 * (i - 15 * page), fileName);//绘制得分

    return 1;
}
```
### 打包与安装
#### 添加扩展
使用 Microsoft Visual Studio - 扩展 - 管理扩展 来添加 Microsoft Visual Studio Installer Project 2022 组件来快速执行打包操作。
#### 添加主输出文件
Application Folder - Add - 项目输出
#### 添加桌面快捷方式
主输出文件 - Create Shortcut to... 来生成快捷方式，拖动生成的快捷方式至 User's Desktop 来将设置安装时快捷方式添加到用户桌面。
#### 添加依赖文件
将项目所用到的资源依赖文件拖动到合适的路径。
#### 生成包文件
Project - 生成
### 主要函数
#### main.cpp
```C
/*导入包*/
#include <graphics.h>
#include <stdio.h>
#include <io.h>
#include <stdlib.h>
#include <string.h>
#include <direct.h>
#include <time.h>
#include <malloc.h>
#include <sys/types.h>
#include <dirent.h>
/*宏定义*/
#define windowWidth 460 //初始窗口宽度 460
#define windowHeight 680 //初始窗口高度 680
#define marginTop 50 //页面元素距离顶部距离
#define GREEN10 RGB(114,162,140) //绿色1
#define GREEN20 RGB(236,246,238) //绿色2
#define GREEN30 RGB(225,238,228) //绿色3
#define GREEN40 RGB(33,106,76) //绿色4
#define bkColor RGB(248,253,247) //背景颜色
#define RANKPAGE 3 //游戏得分记录页面
#define ABOUTPAGE 2 //游戏关于页面
#define PLAYPAGE 1 //游戏下棋页面
#define INITPAGE 0 //游戏初始页面
#define REVIEWPAGE 8 //棋局再现页面
#define ERRORPAGE 4 //错误-页面
#define ERRORFIX 5 //错误-修复
#define ERRORCREATFOLDER 6 //错误-创建文件夹
#define ERRORPUTRECORD 7 //错误-写入游戏
#define ERROROPENFOLDER 9 //错误-打开文件夹
#define COMPUTERGAME 0 //人机对战模式
#define DOUBLEGAME 1 //双人对战模式
#define ME 0 //己方
#define YOU 1 //对方
#define EOL 10 //End of List
#define HOL 11 //Head of List
/*全局变量*/
int mouseClickX = 0; //鼠标点击位置对应的棋盘坐标X
int mouseClickY = 0; //鼠标点击位置对应的棋盘坐标X
int canContinue = 0; //是否可以继续
int computerX = 0; //电脑下棋位置X
int computerY = 0; //电脑下棋位置Y
int doubleX = 0; //双人模式下对方下棋坐标X
int doubleY = 0; //双人模式下对方下棋坐标X
int bpX = 0; //（用于5子相连红线绘制）起始像素坐标X
int bpY = 0; //（用于5子相连红线绘制）起始像素坐标Y
int epX = 0; //（用于5子相连红线绘制）结束像素坐标X
int epY = 0; //（用于5子相连红线绘制）结束像素坐标Y
int state[15][15] = { 0 };  //状态数组_玩家1，对方2，空0
int weight[15][15] = { 0 }; //权重数组_用于人机对战电脑下棋
int score = 0; //得分_每下一次棋加一分，赢再加30分
char kpath[200]; //（用于棋局再现）本局游戏文件记录位置
int rwho = 0; //（用于棋局再现）本步棋谁下的
int rx = 0; //（用于棋局再现）下棋棋盘坐标X
int ry = 0; //（用于棋局再现）下棋棋盘坐标X
/*函数声明*/
int windowInit(int x); //初始化页面
int dataInit(); //初始化数据
int initPage(); //起始页面
int transferBeginToPxy(int x, int y); //（用于5子相连红线绘制）将起始棋盘坐标转化为像素坐标
int transferEndToPxy(int x, int y); //（用于5子相连红线绘制）将终止棋盘坐标转化为像素坐标
int drawChessBoard(); //绘制棋盘
int drawFiveLinkLine(int bx, int by, int ex, int ey); //绘制5子相连红线
int drawMyChess(int x, int y); //绘制己方棋子
int drawYourChess(int x, int y); //绘制对方棋子
int drawComputerChess(); //绘制电脑棋子
int drawAboutPage(); //绘制关于页面
int drawEndPage(int x); //绘制结束页面
int drawRankPage(int x); //绘制游戏得分记录页面
int drawRankItems(int rank, char value[]); //绘制游戏得分记录页面得分项目
int changeWeight(int x, int y); //改变权重
int isFiveLink(int x, int y); //判断自己是否5子相连
int isYouFiveLink(int x, int y); //判断对方是否5子相连
int isChessCanDown(int x, int y, int z); //判断该位置能否下棋
int getMouseClick(int x, int y, int z); //获取鼠标点击像素位置
int getMouseClickBtn(int x, int y, int page); //获取鼠标点击按钮
int getRankFile(); //读取游戏得分记录
int putRankFile(); //提交游戏得分记录
int clearRankFile(); //清空游戏得分记录
int clickEndBtn(); //点击结束按钮
int newComputerGame(); //新人机对战游戏
int newDoubleGame(); //新双人对战游戏
int creatMessage(int Page); //创建弹窗
int setHwnd(); //设置窗口句柄
char* creatPlayRecord(int mode); //创建游戏记录文件
int putPlayRecord(int x, int y, int who); //提交游戏记录
int drawRecordFileList(); //绘制棋局再现列表页面框架
int drawReviewPageBtn(); //绘制棋局再现页面按钮
int drawRecordFileListItem(int i, int page, char fileName[]); //绘制棋局再现页面列表项目
int recoveryByPlayRecord(const char kpath[]); //通过游戏记录文件再现棋盘
/*功能函数*/
/*设置窗口句柄*/
int setHwnd() {
    HWND hnd = GetHWnd();
    SetWindowText(hnd, "五子棋_C语言小组作业");
    return 1;
}
/*初始化界面*/
int windowInit(int x) {
    switch (x) {
    case RANKPAGE: {//记录界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口（460+20，640，禁止窗口最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色
        cleardevice();//清屏
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(38, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "游戏记录");//输出文字（X位置，Y位置，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "记录您最近10次游戏的得分");//输出文字（X，Y，文字）
        break;
    }
    case ABOUTPAGE: {//关于界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);  //创建窗口（宽度，高度，禁止最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（背景色RGB）
        cleardevice();//清屏        
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(38, 0, "微软雅黑");//设置文字样式（高度，间隔，字体） 
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "关于");//输出文字（X，Y，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "浙江工业大学_程序设计基础C语言作业");//输出文字（X，Y，文字）
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 95, "小组成员");//输出文字（X，Y，文字）
        settextstyle(26, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）          
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 130, "白展硕_302023510113_应用功能/算法/文档");//输出文字（X，Y，文字）       
        outtextxy(40, 155, "章方舟_302023510106");//输出文字（X，Y，文字）         
        outtextxy(40, 180, "杨   晨_302023510125_文档");//输出文字（X，Y，文字）         
        outtextxy(40, 205, "龙   勇_302023510119_注释");//输出文字（X，Y，文字）         
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 240, "应用介绍");//输出文字（X，Y，文字）
        settextstyle(26, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(40, 275, "该应用采用C语言编写，其有两种游戏模式，");//输出文字（X，Y，文字）         
        outtextxy(40, 300, "双人游戏可以两个真人分别落子下棋；");//输出文字（X，Y，文字）         
        outtextxy(40, 325, "人机对战可以让你和机器人下棋对战。");//输出文字（X，Y，文字）       
        settextcolor(BLUE);
        outtextxy(40, 350, "浏览更多...");//输出文字（X，Y，文字       
        setfillcolor(GREEN30);//设置填充颜色（RGB）
        setlinecolor(GREEN30);//设置线条颜色（RGB）
        fillroundrect(170, 450, 290, 490, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
        setbkmode(TRANSPARENT);//设置背景模式（透明）        
        settextcolor(GREEN40);//设置文字颜色（RGB）
        outtextxy(208, 454, "返回");;//输出文字（X，Y，文字）         
        setlinecolor(GREEN10);//设置线条颜色（RGB）
        while (1) {
            ////printf("WW");
            if (MouseHit()) {//点击鼠标操作
                MOUSEMSG msg = GetMouseMsg();//定义
                switch (msg.uMsg) {//结构体
                case WM_LBUTTONDOWN://点击左键
                    //printf("%d,%d", msg.x, msg.y);
                    getMouseClickBtn(msg.x, msg.y, ABOUTPAGE);//获取鼠标位置

                }
            }
        }

        break;
    }
    case PLAYPAGE: {//对战界面左上的“五子棋”
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口  
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（RGB）                                   
        cleardevice();//清屏                                       
        settextcolor(GREEN40);//设置文字颜色（RGB）                      
        settextstyle(25, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT); //设置背景模式（透明）                  
        outtextxy(20, 10, "五子棋");//输出文字       
        drawChessBoard();//边框
        break;
    };
    case INITPAGE: {//进入APP初始界面
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//设置背景颜色（RGB） 
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色（RGB）                                   
        cleardevice();//清屏                                       
        settextcolor(GREEN40);//设置文字颜色（RGB                      
        settextstyle(40, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(185, 170, "五子棋");//输出文字         ;
        setfillcolor(GREEN30);//设置填充颜色
        setlinecolor(GREEN30);//设置线条颜色
        fillroundrect(170, 330, 290, 370, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(188, 334, "双人游戏");//输出文字         
        fillroundrect(170, 390, 290, 430, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式           
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(188, 394, "人机对战");//输出文字          
        fillroundrect(170, 450, 290, 490, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色                      
        settextstyle(30, 0, "微软雅黑");//设置文字样式            
        setbkmode(TRANSPARENT);//设置背景模式（透明）                   
        outtextxy(210, 454, "关于");//输出文字 
        fillroundrect(170, 510, 290, 550, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(188, 514, "得分记录");//输出文字 
        fillroundrect(170, 570, 290, 610, 40, 40);//圆角
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(188, 574, "棋局再现");//输出文字 
        setlinecolor(GREEN10);//设置线条颜色
        break;
    };
    case REVIEWPAGE: {
        initgraph(windowWidth + 20, windowHeight, NOMINIMIZE);//创建窗口（460+20，640，禁止窗口最小化）
        setHwnd();
        setbkcolor(bkColor);//设置背景颜色
        cleardevice();//清屏
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(38, 0, "微软雅黑");//设置文字样式
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
        setbkmode(TRANSPARENT);//设置背景模式（透明）
        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）

    }
    }
    return 1;
}
/*数据初始化*/
int dataInit() {//棋盘初始化
    for (int i = 0; i < 15; i++) {
        for (int k = 0; k < 15; k++) {
            state[i][k] = 0;//清除所有已下棋子
        }
    }
    for (int i = 0; i < 10; i++) {
        for (int k = 0; k < 10; k++) {
            weight[i][k] = 0;//清除所有权重
        }
    }
    score = 0;//得分恢复为0
    return 1;
}
/*创建初始界面*/
int initPage() {
    windowInit(INITPAGE);//初始化所有基本界面
    while (1) {//多次获取鼠标当前位置
        if (MouseHit()) {//点击鼠标运行
            MOUSEMSG msg = GetMouseMsg();//定义，获取鼠标位置
            switch (msg.uMsg) {//选择语句
            case WM_LBUTTONDOWN://点击鼠标左键
                getMouseClickBtn(msg.x, msg.y, INITPAGE);//获取鼠标点击的坐标
            }
        }
    }
    return 1;
}
/*将鼠标位置转化为在棋盘线的开始坐标*/
int transferBeginToPxy(int x, int y) {//将鼠标位置转化为在棋盘线的开始坐标
    bpX = 50 + 40 * x;
    bpY = 50 + marginTop + 40 * y;
    return 1;
}
/*将鼠标位置转化为在棋盘线的结束坐标*/
int transferEndToPxy(int x, int y) {//将鼠标位置转化为在棋盘线的结束坐标
    epX = 50 + 40 * x;
    epY = 50 + marginTop + 40 * y;
    return 1;
}
/*制作棋盘基本框架*/
int drawChessBoard() {
    //边框
    setlinestyle(PS_SOLID, 3);//设置线条样式
    setlinecolor(GREEN40);//设置线条颜色
    line(10, 10 + marginTop, 10, 450 + marginTop);
    line(10, 10 + marginTop, windowWidth - 10, 10 + marginTop);
    line(windowWidth - 10, 10 + marginTop, windowWidth - 10, 450 + marginTop);
    line(10, 450 + marginTop, windowWidth - 10, 450 + marginTop);
    //10*10表格
    setlinestyle(PS_SOLID, 2);
    setlinecolor(GREEN10);
    for (int i = 0; i < 10; i++) {
        line(10, 50 + 40 * i + marginTop, windowWidth - 10, 50 + 40 * i + marginTop);
    }
    for (int i = 0; i < 10; i++) {
        line(50 + 40 * i, 10 + marginTop, 50 + 40 * i, 450 + marginTop);
    }
    //文字标号
    settextcolor(GREEN40);
    settextstyle(20, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    outtextxy(45, marginTop - 10, "1        2        3        4         5        6        7       8         9       10");
    for (int i = 0; i < 10; i++) {
        outtextxy(460, marginTop + 40 + 40 * i, 'a' + i);
    }
    return 1;
}
/*画五个棋连在一起的线*/
int drawFiveLinkLine(int bx, int by, int ex, int ey) {//五个连一起画线
    transferBeginToPxy(bx, by);//将鼠标位置转化为在棋盘线的开始坐标
    transferEndToPxy(ex, ey);//将鼠标位置转化为在棋盘线的结束坐标
    setlinestyle(PS_SOLID, 3);//设置线条样式
    setlinecolor(RED);//设置线条颜色
    line(bpX, bpY, epX, epY);//画一条线
    setlinecolor(GREEN10);//设置线条颜色
    setlinestyle(PS_SOLID, 2);//设置线条样式
    return 1;
}
/*玩家下棋*/
int drawMyChess(int x, int y) {//下我的棋子
    setfillcolor(GREEN40);
    fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
    state[x][y] = 1;
    score++;
    changeWeight(x, y);//改变权重
    canContinue = 1;
    putPlayRecord( x, y, ME);
    return 1;
}
/*对方下的棋*/
int drawYourChess(int x, int y) {//下对方的棋子
    setfillcolor(bkColor);
    fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
    doubleX = x;
    doubleY = y;
    state[x][y] = 2;
    canContinue = 0;
    putPlayRecord( x, y, YOU);
    return 1;
}
/*电脑下棋*/
int drawComputerChess() {
    //初始化下棋坐标
    int x = 0;
    int y = 0;
    computerX = 0;
    computerY = 0;
    //初始化是否找到下棋位置
    int isFind = 0;
    int max = 0;
    for (int i = 0; i < 10; i++) {
        for (int k = 0; k < 10; k++) {
            if (weight[i][k] > max&& state[i][k] == 0) {
                max = weight[i][k];
            }
        }
    }
    for (int i = 0; i < 10; i++) {
        for (int k = 9; k >= 0; k--) {
            if (weight[i][k] == max && state[i][k] == 0) {
                x = i;
                y = k;
                isFind = 1;
                i + 10;

            }
        }
    }
    while (isFind != 1) {
        srand((unsigned int)time(NULL));
        int ret1 = rand() % 10;
        int ret2 = rand() % 10;
        x = ret1;
        y = ret2;
        if (state[x][y] == 0) {
            isFind = 1;
        }
    }
    if (state[x][y] == 0) {
        setfillcolor(bkColor);
        fillcircle(50 + 40 * x, 50 + marginTop + 40 * y, 10);
        computerX = x;
        computerY = y;
        state[x][y] = 2;
        changeWeight(x, y);
        canContinue = 0;
        putPlayRecord( x, y, YOU);
        return 1;
    }
    else {
        //printf("ERROR\n");
    }

}
/*页面结算*/
int drawEndPage(int x) {//本局结束页面
    setfillcolor(GREEN30);//设置填充颜色
    setlinecolor(GREEN30);//设置线条颜色
    fillroundrect(170, 530, 290, 570, 40, 40);//有填充的圆角
    if (x) {
        settextcolor(GREEN40);//设置文字颜色
        settextstyle(30, 0, "微软雅黑");//文字样式
        setbkmode(TRANSPARENT);//背景模式
        outtextxy(195, 534, "你赢了！");//输出文字
    }
    else {
        settextcolor(GREEN40);//文字颜色
        settextstyle(30, 0, "微软雅黑");//文字样式
        setbkmode(TRANSPARENT);//背景模式
        outtextxy(195, 534, "对方胜！");//输出文字
    }
    putRankFile();//提交得分
    //printf("%d\n", score);
    /*绘制确定按钮*/
    settextcolor(GREEN40);
    settextstyle(22, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    outtextxy(214, 575, "确定");
    return 1;
}
/*绘制得分记录界面框架*/
int drawRankPage(int x) {
    switch (x) {
    case 1: {
        /*绘制返回按钮*/
        setfillcolor(GREEN30);
        setlinecolor(GREEN30);
        fillroundrect(170, 450, 290, 490, 40, 40);
        settextcolor(GREEN40);
        settextstyle(30, 0, "微软雅黑");
        setbkmode(TRANSPARENT);
        outtextxy(208, 454, "返回");
        setlinecolor(GREEN10);
        /*绘制清空按钮*/
        setfillcolor(GREEN30);
        setlinecolor(GREEN30);
        fillroundrect(170, 510, 290, 550, 40, 40);
        settextcolor(GREEN40);
        settextstyle(30, 0, "微软雅黑");
        setbkmode(TRANSPARENT);
        outtextxy(208, 514, "清空");
        setlinecolor(GREEN10);
        /*判断鼠标点击返回按钮*/
        while (1) {
            ////printf("WW");
            if (MouseHit()) {
                MOUSEMSG msg = GetMouseMsg();
                switch (msg.uMsg) {
                case WM_LBUTTONDOWN:
                    //printf("%d,%d", msg.x, msg.y);
                    getMouseClickBtn(msg.x, msg.y, RANKPAGE);
                }
            }
        }
        break;
    }
    case 0: {
        windowInit(RANKPAGE);//初始化窗口
        getRankFile();//获取得分记录
    }
    }
    return 1;
}
/*绘制得分记录界面内容*/
int drawRankItems(int rank, char value[]) {
    settextstyle(26, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    char str[10];
    itoa(rank, str, 10);
    outtextxy(40, 95 + 25 * (rank - 1), str);//绘制序号
    outtextxy(120, 95 + 25 * (rank - 1), strcat(value, "分"));//绘制得分
    return 1;
}
/*改变权重*/
int changeWeight(int x, int y) {
    int Score[3][5] = {
        { 0, 20, 360, 5800, 92840 }, // 防守0子
        { 0, 0, 20, 360, 92840 },    // 防守1子
        { 0, 0, 0, 0, 92840 }        // 防守2子
    };
    int dx[4]{ 1,0,1,1 }; // - | \ / 四个方向
    int dy[4]{ 0,1,1,-1 };

    int nowi = 0;               // 现在遍历到的y坐标
    int nowj = 0;               // 现在遍历到的x坐标
    int length[4];              // 四个方向的长度
    int emeny[4];               // 四个方向的敌子

    for (int i = 0; i < 10; i++)
    {
        for (int j = 0; j < 10; j++)
        {
            if (state[i][j] == 0)
            {
                // 遍历每一个可能的位置

                // 自己
                state[i][j] = 2; // 尝试下在这里
                for (int k = 0; k < 4; k++)
                {
                    length[k] = 0;
                    emeny[k] = 0;
                    nowi = i;
                    nowj = j;
                    while (nowi <= 9 && nowj <= 9 && nowi >= 0 && nowj >= 0 && state[nowi][nowj] == 2)
                    {
                        length[k]++;
                        nowj += dx[k];
                        nowi += dy[k];
                    }
                    if (state[nowi][nowj] == 1  || nowi < 0 || nowj < 0 || nowi > 9 || nowj > 9)
                    {
                        emeny[k]++;
                    }
                    nowi = i;
                    nowj = j;
                    while (nowi <= 9 && nowj <= 9 && nowi >= 0 && nowj >= 0 && state[nowi][nowj] == 2)
                    {
                        length[k]++;
                        nowj -= dx[k];
                        nowi -= dy[k];
                    }
                    if (state[nowi][nowj] == 1 || nowi < 0 || nowj < 0 || nowi > 9 || nowj > 9)
                    {
                        emeny[k]++;
                    }
                    length[k] -= 2; // 判断长度
                    if (length[k] > 4)
                    {
                        length[k] = 4;
                    }
                    weight[i][j] += Score[emeny[k]][length[k]] * 4 + !(!length[k]) * 20;//加分系统
                    length[k] = 0;
                    emeny[k] = 0;
                }
                // 敌人（原理同上）
                state[i][j] = 1;
                for (int k = 0; k < 4; k++)
                {
                    length[k] = 0;
                    emeny[k] = 0;
                    nowi = i;
                    nowj = j;
                    while (nowi <= 9 && nowj <= 9 && nowi >= 0 && nowj >= 0 && state[nowi][nowj] == 1 )
                    {
                        length[k]++;
                        nowj += dx[k];
                        nowi += dy[k];
                    }
                    if (state[nowi][nowj] == 2 || nowi < 0 || nowj < 0 || nowi > 9 || nowj > 9)
                    {
                        emeny[k]++;
                    }
                    nowi = i;
                    nowj = j;
                    while (nowi <= 9 && nowj <= 9 && nowi >= 0 && nowj >= 0 && state[nowi][nowj] == 1 )
                    {
                        length[k]++;
                        nowj -= dx[k];
                        nowi -= dy[k];
                    }
                    if (state[nowi][nowj] == 2 || nowi < 0 || nowj < 0 || nowi > 9 || nowj > 9)
                    {
                        emeny[k]++;
                    }
                    length[k] -= 2;
                    if (length[k] > 4)
                    {
                        length[k] = 4;
                    }
                    weight[i][j] += Score[emeny[k]][length[k]];
                    length[k] = 0;
                    emeny[k] = 0;
                }
                state[i][j] = 0;
            }
        }
    }
    return 1;
}
/*5连_玩家能否赢的算法*/
int isFiveLink(int x, int y) {//是否玩家五个连成一条线（判断是否游戏结束算法）drawFiveLinkLine即画线
    //oooo_（最右边差一个）
    //printf("%d %d\n",x,y);
    if (state[x - 1][y] == 1 && state[x - 2][y] == 1 && state[x - 3][y] == 1 && state[x - 4][y] == 1) {
        drawFiveLinkLine(x, y, x - 4, y);
        return 1;
    }
    //ooo_o（中间差一个）
    else if (state[x + 1][y] == 1 && state[x - 1][y] == 1 && state[x - 2][y] == 1 && state[x - 3][y] == 1) {
        drawFiveLinkLine(x + 1, y, x - 3, y);
        return 1;
    }
    //oo_oo（中间差一个）
    else if (state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x - 1][y] == 1 && state[x - 2][y] == 1) {
        drawFiveLinkLine(x + 1, y, x - 2, y);
        return 1;
    }
    //o_ooo（中间差一个）
    else if (state[x - 1][y] == 1 && state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x + 3][y] == 1) {
        drawFiveLinkLine(x - 1, y, x + 3, y);
        return 1;
    }
    //_oooo（最左边差一个）
    else if (state[x + 1][y] == 1 && state[x + 2][y] == 1 && state[x + 3][y] == 1 && state[x + 4][y] == 1) {
        drawFiveLinkLine(x, y, x + 4, y);
        return 1;
    }
    //oooo__y（竖的方向最下面差一个）
    else if (state[x][y - 1] == 1 && state[x][y - 2] == 1 && state[x][y - 3] == 1 && state[x][y - 4] == 1) {
        drawFiveLinkLine(x, y, x, y - 4);
        return 1;
    }
    //ooo_o_y（竖的方向中间差一个）
    else if (state[x][y + 1] == 1 && state[x][y - 1] == 1 && state[x][y - 2] == 1 && state[x][y - 3] == 1) {
        drawFiveLinkLine(x, y + 1, x, y - 3);
        return 1;
    }
    //oo_oo_y（竖的方向中间差一个）
    else if (state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y - 1] == 1 && state[x][y - 2] == 1) {
        drawFiveLinkLine(x, y + 2, x, y - 2);
        return 1;
    }
    //o_ooo_（竖的方向中间差一个）
    else if (state[x][y - 1] == 1 && state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y + 3] == 1) {
        drawFiveLinkLine(x, y - 1, x, y + 3);
        return 1;
    }
    //_oooo_y（竖的方向最上面差一个）
    else if (state[x][y + 1] == 1 && state[x][y + 2] == 1 && state[x][y + 3] == 1 && state[x][y + 4] == 1) {
        drawFiveLinkLine(x, y, x, y + 4);
        return 1;
    }
    //oooo__left|top_to_right|bottom（左上到右下差一个）
    else if (state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1 && state[x - 3][y - 3] == 1 && state[x - 4][y - 4] == 1) {
        drawFiveLinkLine(x, y, x - 4, y - 4);
        return 1;
    }
    //ooo_o_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x + 1][y + 1] == 1 && state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1 && state[x - 3][y - 3] == 1) {
        drawFiveLinkLine(x + 1, y + 1, x - 3, y - 3);
        return 1;
    }
    //oo_oo_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x - 1][y - 1] == 1 && state[x - 2][y - 2] == 1) {
        drawFiveLinkLine(x + 2, y + 2, x - 2, y - 2);
        return 1;
    }
    //o_ooo_left|top_to_right|bottom（左上到右下中间差一个）
    else if (state[x - 1][y - 1] == 1 && state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x + 3][y + 3] == 1) {
        return 1;
    }
    //_oooo_left|top_to_right|bottom（左上到右下差一个）
    else if (state[x + 1][y + 1] == 1 && state[x + 2][y + 2] == 1 && state[x + 3][y + 3] == 1 && state[x + 4][y + 4] == 1) {
        drawFiveLinkLine(x, y, x + 4, y + 4);
        return 1;
    }
    //oooo__left|bottom_to_right|top（左下到右上差一个）
    else if (state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1 && state[x - 3][y + 3] == 1 && state[x - 4][y + 4] == 1) {
        drawFiveLinkLine(x, y, x - 4, y + 4);
        return 1;
    }
    //ooo_o_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x + 1][y - 1] == 1 && state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1 && state[x - 3][y + 3] == 1) {
        drawFiveLinkLine(x + 1, y - 1, x - 3, y + 3);
        return 1;
    }
    //oo_oo_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x - 1][y + 1] == 1 && state[x - 2][y + 2] == 1) {
        drawFiveLinkLine(x + 2, y - 2, x - 2, y + 2);
        return 1;
    }
    //o_ooo_left|bottom_to_right|top（左下到右上中间差一个）
    else if (state[x - 1][y + 1] == 1 && state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x + 3][y - 3] == 1) {
        drawFiveLinkLine(x + 3, y - 3, x - 1, y + 1);
        return 1;
    }
    //_oooo_left|bottom_to_right|top（左下到右上差一个）
    else if (state[x + 1][y - 1] == 1 && state[x + 2][y - 2] == 1 && state[x + 3][y - 3] == 1 && state[x + 4][y - 4] == 1) {
        drawFiveLinkLine(x + 4, y - 4, x, y);
        return 1;
    }

    return 0;
}
/*5连_电脑/对方能否赢的算法*/
int isYouFiveLink(int x, int y) {//以下为判断电脑是否胜的算法
    //oooo_
    //printf("===%d %d\n", x, y);
    if (state[x - 1][y] == 2 && state[x - 2][y] == 2 && state[x - 3][y] == 2 && state[x - 4][y] == 2) {
        drawFiveLinkLine(x, y, x - 4, y);
        return 1;
    }
    //ooo_o
    else if (state[x + 1][y] == 2 && state[x - 1][y] == 2 && state[x - 2][y] == 2 && state[x - 3][y] == 2) {
        drawFiveLinkLine(x + 1, y, x - 3, y);
        return 1;
    }
    //oo_oo
    else if (state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x - 1][y] == 2 && state[x - 2][y] == 2) {
        drawFiveLinkLine(x + 1, y, x - 2, y);
        return 1;
    }
    //o_ooo
    else if (state[x - 1][y] == 2 && state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x + 3][y] == 2) {
        drawFiveLinkLine(x - 1, y, x + 3, y);
        return 1;
    }
    //_oooo
    else if (state[x + 1][y] == 2 && state[x + 2][y] == 2 && state[x + 3][y] == 2 && state[x + 4][y] == 2) {
        drawFiveLinkLine(x, y, x + 4, y);
        return 1;
    }
    //oooo__y
    else if (state[x][y - 1] == 2 && state[x][y - 2] == 2 && state[x][y - 3] == 2 && state[x][y - 4] == 2) {
        drawFiveLinkLine(x, y, x, y - 4);
        return 1;
    }
    //ooo_o_y
    else if (state[x][y + 1] == 2 && state[x][y - 1] == 2 && state[x][y - 2] == 2 && state[x][y - 3] == 2) {
        drawFiveLinkLine(x, y + 1, x, y - 3);
        return 1;
    }
    //oo_oo_y
    else if (state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y - 1] == 2 && state[x][y - 2] == 2) {
        drawFiveLinkLine(x, y + 2, x, y - 2);
        return 1;
    }
    //o_ooo_y
    else if (state[x][y - 1] == 2 && state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y + 3] == 2) {
        drawFiveLinkLine(x, y - 1, x, y + 3);
        return 1;
    }
    //_oooo_y
    else if (state[x][y + 1] == 2 && state[x][y + 2] == 2 && state[x][y + 3] == 2 && state[x][y + 4] == 2) {
        drawFiveLinkLine(x, y, x, y + 4);
        return 1;
    }
    //oooo__left|top_to_right|bottom
    else if (state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2 && state[x - 3][y - 3] == 2 && state[x - 4][y - 4] == 2) {
        drawFiveLinkLine(x, y, x - 4, y - 4);
        return 1;
    }
    //ooo_o_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2 && state[x - 3][y - 3] == 2) {
        drawFiveLinkLine(x + 1, y + 1, x - 3, y - 3);
        return 1;
    }
    //oo_oo_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x - 1][y - 1] == 2 && state[x - 2][y - 2] == 2) {
        drawFiveLinkLine(x + 2, y + 2, x - 2, y - 2);
        return 1;
    }
    //o_ooo_left|top_to_right|bottom
    else if (state[x - 1][y - 1] == 2 && state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x + 3][y + 3] == 2) {
        return 1;
    }
    //_oooo_left|top_to_right|bottom
    else if (state[x + 1][y + 1] == 2 && state[x + 2][y + 2] == 2 && state[x + 3][y + 3] == 2 && state[x + 4][y + 4] == 2) {
        drawFiveLinkLine(x, y, x + 4, y + 4);
        return 1;
    }
    //oooo__left|bottom_to_right|top
    else if (state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2 && state[x - 3][y + 3] == 2 && state[x - 4][y + 4] == 2) {
        drawFiveLinkLine(x, y, x - 4, y + 4);
        return 1;
    }
    //ooo_o_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2 && state[x - 3][y + 3] == 2) {
        drawFiveLinkLine(x + 1, y - 1, x - 3, y + 3);
        return 1;
    }
    //oo_oo_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x - 1][y + 1] == 2 && state[x - 2][y + 2] == 2) {
        drawFiveLinkLine(x + 2, y - 2, x - 2, y + 2);
        return 1;
    }
    //o_ooo_left|bottom_to_right|top
    else if (state[x - 1][y + 1] == 2 && state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x + 3][y - 3] == 2) {
        drawFiveLinkLine(x + 3, y - 3, x - 1, y + 1);
        return 1;
    }
    //_oooo_left|bottom_to_right|top
    else if (state[x + 1][y - 1] == 2 && state[x + 2][y - 2] == 2 && state[x + 3][y - 3] == 2 && state[x + 4][y - 4] == 2) {
        drawFiveLinkLine(x + 4, y - 4, x, y);
        return 1;
    }
    return 0;
}
/*能否在此处下棋*/
int isChessCanDown(int x, int y, int z) {//该位置是否可以下棋
    switch (state[x][y]) {
    case 0://如果没有棋子，则绘制棋子并返回0
        switch (z) {
        case 1:drawMyChess(x, y); break;//下玩家的棋子
        case 0:drawYourChess(x, y);//下对方的棋子
        }
        return 0;
        break;
    case 1://如果有己方棋子，则返回1
        switch (z) {
        case 1:canContinue = 0; break;//对方不能下棋
        case 0:canContinue = 1; break;//对方可以下棋
        }
        return 1;
    case 2://如果有对方棋子，则返回2
        switch (z) {
        case 1:canContinue = 0; break;
        case 0:canContinue = 1; break;
        }
        return 2;
    default://出错，返回-1
        switch (z) {
        case 1:canContinue = 0; break;
        case 0:canContinue = 1; break;
        }
        return -1;
    }
}
/*下棋时鼠标点的位置*/
int getMouseClick(int x, int y, int z) {//获取鼠标点击位置
    for (int i = 0; i < 10; i++) {
        for (int k = 0; k < 10; k++) {
            if (x > (50 + 40 * k - 15) && x<(50 + 40 * k + 15) && y>(marginTop + 50 + 40 * i - 15) && y < (marginTop + 50 + 40 * i + 15)) {
                mouseClickX = k;
                mouseClickY = i;
                isChessCanDown(mouseClickX, mouseClickY, z);  //棋子是否可以下
                return 1;
            }
        }
    }
}
/*鼠标在哪个界面进行什么操作*/
int getMouseClickBtn(int x, int y, int page) {//在第几个页面操作并获取鼠标进行的操作
    switch (page) {
    case INITPAGE: {//都是获取鼠标点击的位置判断要进行什么操作
        if (x < 290 && x>170 && y > 390 && y < 430) {
            newComputerGame();//人机对战
        }
        else if (x < 290 && x>170 && y > 330 && y < 370) {
            newDoubleGame();//双人对战
        }
        else if (x < 290 && x>170 && y > 450 && y < 490) {
            drawAboutPage();//关于
        }
        else if (x < 290 && x>170 && y > 510 && y < 550) {
            drawRankPage(0);//记录
        }
        else if (x < 290 && x>170 && y > 570 && y < 610) {
            drawRecordFileList();
        }
        break;
    }
    case ABOUTPAGE: {
        if (x < 290 && x>170 && y > 450 && y < 490) {
            initPage();//再次打开界面
            closegraph();//关闭界面
        }
        else if (x>40 && x<165&&y>350&&y<376) {
            system("explorer https://p.kdocs.cn/s/GPHJUBIAJE");
        }
        break;
    }
    case RANKPAGE: {
        if (x < 290 && x>170 && y > 450 && y < 490) {
            initPage();//再次打开界面
            closegraph();//关闭界面
        }
        else if (x < 290 && x>170 && y > 510 && y < 550) {
            creatMessage(RANKPAGE);
        }
        break;
    }
    case REVIEWPAGE: {
        if (x < 290 && x>170 && y > 550 && y < 590) {
            initPage();
            closegraph();//关闭界面
            return -1;
        }
        else if (x < 150 && x>30 && y > 550 && y < 590) {
            return -3;
        }
        else if (x < 430 && x>310 && y > 550 && y < 590) {
            return -2;
        }
        else {
            for (int i = 0; i < 15; i++) {
                if (y > 120 + 25 * i && y < 120 + 25 * i + 26) {
                    //printf("%d",i);
                    return i;
                }

            }
            //printf("==%d,%d==",x,y );
            return -4;


        }



        break;
    }
    }
    return 1;
}
/*获取得分记录*/
int getRankFile() {
    if (!access("res", 0)) {
    }

    else {
        creatMessage(ERRORFIX);
        drawRankPage(1);
        return 0;
    }

    if (!access("res/Rank.txt", 0)) {
    }

    else {
        FILE* fp2 = fopen("res/Rank.txt", "w+");
        fputs("0", fp2);
        fclose(fp2);
    }




    FILE* fp = fopen("res/Rank.txt", "r");
    /*判空*/
    if (fp == NULL) {
        creatMessage(ERRORPAGE);
        //printf("请使用管理员模式打开程序\n");
        return 0;
    }

    /*读取_传参_绘制*/
    char a[10];
    fgets(a, 10, fp);
    int b = atoi(a);
    //printf("%d\n", b);
    char content[100] = { 0 };
    if (b == 0) {
        settextcolor(GREEN40);
        settextstyle(30, 0, "微软雅黑");
        setbkmode(TRANSPARENT);
        outtextxy(190, 170, "暂无记录");
    }
    else if (b > 10) {
        for (int i = 0; i < 10; i++) {
            fgets(a, 10, fp);
            drawRankItems(i + 1, a);
        }
    }
    else {
        for (int i = 0; i < b; i++) {
            fgets(a, 10, fp);
            drawRankItems(i + 1, a);
        }
    }
    drawRankPage(1);

    /*关闭文件*/
    fclose(fp);
    return 1;
}
/*提交得分记录*/
int putRankFile() {
    if (!access("res", 0)) {
    }

    else {
        creatMessage(ERRORFIX);
        return 0;
    }
    if (!access("res/Rank.txt", 0)) {
    }

    else {
        FILE* fp2 = fopen("res/Rank.txt", "w+");
        fputs("0", fp2);
        fclose(fp2);
    }
    /*先读*/

    FILE* fp = fopen("res/Rank.txt", "r");
    /*判空*/
    if (fp == NULL) {
        creatMessage(ERRORPAGE);
        //printf("请使用管理员模式打开程序\n");
        return 0;
    }
    /*整合数据*/
    char content[100][100] = { 0 };
    char line[10] = { 0 };
    fgets(line, 10, fp);
    int newLine = atoi(line);
    for (int i = 0; i < newLine; i++) {
        fgets(content[i], 100, fp);
    }
    itoa(newLine + 1, line, 10);
    fclose(fp);
    /*再写*/
    FILE* fp1 = fopen("res/Rank.txt", "w");
    itoa(newLine + 1, line, 10);
    char str[10] = { 0 };
    if (newLine == 0) {

    }
    else {
        strcat(line, "\n");
    }
    fputs(line, fp1);
    for (int i = 0; i < newLine; i++) {
        fputs(content[i], fp1);
    }
    itoa(score, str, 10);
    fputs("\n", fp1);
    fputs(str, fp1);
    fclose(fp1);
    return 1;
}
/*清空得分记录*/
int clearRankFile() {
    if (!access("res", 0)) {
    }

    else {
        creatMessage(ERRORFIX);
        return 0;
    }
    if (!access("res/Rank.txt", 0)) {
    }

    else {
        FILE* fp2 = fopen("res/Rank.txt", "w+");
        fputs("0", fp2);
        fclose(fp2);
    }
    FILE* fp1 = fopen("res/Rank.txt", "w");
    fputs("0", fp1);
    fclose(fp1);
    initPage();//再次打开界面
    closegraph();//关闭界面
    return 1;
}
/*NEW:创建游戏记录文件*/
char* creatPlayRecord(int mode) {
    int res;
    if (!access("record", 0)) {
    }
    else {
        res = mkdir("record");// 返回 0 表示创建成功，-1 表示失败
        if (res == 0) {
        }
        else {
            creatMessage(ERRORCREATFOLDER);
        }
    }
    time_t timep;
    struct tm* p;
    //static char kpath[200] = {};
    char content[200] = {};
    time(&timep);
    p = gmtime(&timep);
    sprintf(kpath, "record/%d%02d%02d_%02d%02d%02d.txt\0", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
    switch (mode) {
    case COMPUTERGAME: {
        sprintf(content, "record/%d%02d%02d_%02d%02d%02d.txt\n人机模式\n", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
        break;
    }
    case DOUBLEGAME: {
        sprintf(content, "record/%d%02d%02d_%02d%02d%02d.txt\n双人模式\n", 1900 + p->tm_year, 1 + p->tm_mon, p->tm_mday, 8 + p->tm_hour, p->tm_min, p->tm_sec);
        break;

    }
    }
    FILE* fp = fopen(kpath, "w+");
    fputs(content, fp);
    fclose(fp);
    return kpath;
}
/*NEW:更新游戏记录文件*/
int putPlayRecord(int x, int y, int who) {
    //printf("put:%s\n",kpath);
    if (!access(kpath, 0)) {
    }
    else {
       // printf("=============\n");
        creatMessage(ERRORPUTRECORD);
    }
    FILE* fp = fopen(kpath, "a");
    fprintf(fp,"%d:(%d,%d)\n",who,x,y);
    fclose(fp);
    return 1;
}
/*NEW:恢复棋盘*/
int recoveryByPlayRecord(const char kpath[]) {
    dataInit();
    //printf("%s", kpath);
    char fpath[100];
    strcpy(fpath, kpath);
    //printf("%s", fpath);
    if (!access(fpath, 0)) {
    }
    else {
        creatMessage(ERRORPUTRECORD);
    }
    FILE* fp = fopen(fpath, "r");
    char rmode[50] = {};
    char ch[100];
    //printf("\n");
    fgets(ch, 100, fp);
    //printf("%s",ch);
    fgets(ch, 100, fp);
    //printf("%s", ch);
    windowInit(PLAYPAGE);//初始化界面
    while (1) {
        fscanf(fp, "%d:(%d,%d)", &rwho, &rx, &ry);
        if (rwho == ME) {
            setfillcolor(GREEN40);
            fillcircle(50 + 40 * rx, 50 + marginTop + 40 * ry, 10);
            state[rx][ry] = 1;
            isFiveLink(rx, ry);
        }
        else if (rwho == YOU) {
            setfillcolor(bkColor);
            fillcircle(50 + 40 * rx, 50 + marginTop + 40 * ry, 10);
            state[rx][ry] = 2;
            isYouFiveLink(rx, ry);
        }
        else {
            break;
        }
        Sleep(300);
    }
    setfillcolor(GREEN30);//设置填充颜色（RGB）
    setlinecolor(GREEN30);//设置线条颜色（RGB）
    fillroundrect(170, 550, 290, 590, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
    settextcolor(GREEN40);//设置文字颜色（RGB）                      
    settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
    setbkmode(TRANSPARENT);//设置背景模式（透明）                   
    outtextxy(208, 554, "返回");;//输出文字（X，Y，文字）         
    setlinecolor(GREEN10);//设置线条颜色（RGB）
    while (1) {
        if (MouseHit()) {//点击鼠标操作
            MOUSEMSG msg = GetMouseMsg();//定义
            switch (msg.uMsg) {//结构体
            case WM_LBUTTONDOWN://点击左键
                getMouseClickBtn(msg.x, msg.y, REVIEWPAGE);//获取鼠标位置
            }
        }
    }
    fclose(fp);
    return 1;
}
/*弹窗*/
int creatMessage(int Page) {
    switch (Page) {
    case RANKPAGE: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "确定要清空记录？", "提示", MB_OKCANCEL);
        switch (isClickOKBtn) {
        case IDOK: {
            clearRankFile();
            return 1;
        }
        case IDCANCEL: {
            return 0;
        }
        }
    }
    case ERRORPAGE: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "无法写入/读取文件记录，您可能需要右键-管理员模式打开本应用来写入/读取文件记录。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            //clearRankFile();
            return 1;
        }
        }
    }
    case ERRORFIX: {

        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "程序运行文件结构出错，无法找到“res”文件夹，您可尝试重新安装解决问题。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }
    }
    case ERRORCREATFOLDER: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "无法创建文件夹，您可能需要右键-管理员模式打开本应用来写入/读取文件记录。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }
    }
    case ERRORPUTRECORD: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "无法找到文件/无写入/读取文件权限，您可能需要右键-管理员模式打开本应用来写入/读取文件记录。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }
    }
    case ERROROPENFOLDER: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "无法找到/打开“record”文件夹，请检查文件夹是否存在。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }
    }
    case EOL: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "已经是文件列表的最后一页了。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }
    }
    case HOL: {
        HWND hnd = GetHWnd();
        int isClickOKBtn = MessageBox(hnd, "已经是文件列表的第一页了。", "提示", MB_OK);
        switch (isClickOKBtn) {
        case IDOK: {
            return 1;
        }
        }

    }
    }
}
/*点击结束按钮*/
int clickEndBtn() {//点击结束按钮
    while (1) {
        if (MouseHit()) {
            MOUSEMSG msg = GetMouseMsg();//获取鼠标位置
            switch (msg.uMsg) {
            case WM_LBUTTONDOWN://左键
                if (msg.x < 250 && msg.x>190 && msg.y > 565 && msg.y < 610) {
                    dataInit();//棋盘全部初始化
                    initPage();//打开页面
                    closegraph();//关闭页面
                }
            }
        }
    }
    return 1;
}
/*人机对战*/
int newComputerGame() {//人机对战
    windowInit(PLAYPAGE);//初始界面
    dataInit();
    creatPlayRecord(COMPUTERGAME);
    while (1) {
        if (MouseHit()) {
            MOUSEMSG msg = GetMouseMsg();//获取鼠标位置
            switch (msg.uMsg) {
            case WM_LBUTTONDOWN://左键
                getMouseClick(msg.x, msg.y, 1);//鼠标在哪个页面点的什么位置
                if (isFiveLink(mouseClickX, mouseClickY)) {//判断玩家是否五个连成一起
                    putPlayRecord( 0, 0, -1);
                    score += 30;   //加30分
                    drawEndPage(1);//本局玩家结束页面
                    clickEndBtn();//点击结束按钮
                }
                if (canContinue) {
                    canContinue = 0;
                    drawComputerChess();//电脑下棋位置
                    if (isYouFiveLink(computerX, computerY)) {//电脑是否赢
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(0);//本局电脑结束页面
                        clickEndBtn();//点击结束按钮
                    }
                }
                break;
            case WM_RBUTTONDOWN://右键则不操作
                break;
            }
        }
    }
    return 1;
}
/*双人对战*/
int newDoubleGame() {//双人对战
    windowInit(PLAYPAGE);//初始化界面
    dataInit();
    creatPlayRecord(DOUBLEGAME);
    while (1) {
        if (canContinue == 0) {
            if (MouseHit()) {
                MOUSEMSG msg = GetMouseMsg();
                switch (msg.uMsg) {
                case WM_LBUTTONDOWN://左键
                    getMouseClick(msg.x, msg.y, 1);//鼠标点的位置

                    if (isFiveLink(mouseClickX, mouseClickY)) {//第一个玩家是否五个一条线
                        score += 30;//加分
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(1);//画
                        clickEndBtn();//点击结束按钮
                    }


                    break;
                case WM_RBUTTONDOWN://右键
                    break;
                }
            }
        }
        if (canContinue) { //轮到对方操作
            if (MouseHit()) {
                MOUSEMSG msg = GetMouseMsg();
                switch (msg.uMsg) {
                case WM_LBUTTONDOWN:
                    getMouseClick(msg.x, msg.y, 0);//对方鼠标位置
                    if (isYouFiveLink(doubleX, doubleY)) {//对方能否赢
                        putPlayRecord( 0, 0, -1);
                        drawEndPage(0);//画
                        clickEndBtn();//点击结束按钮
                    }
                    break;
                case WM_RBUTTONDOWN:
                    break;
                }
            }
        }

    }
    return 1;
}
/*关于*/
int drawAboutPage() {//关于
    windowInit(ABOUTPAGE);//开辟界面
    return 1;
}
/*NEW:绘制列表项目*/
int drawRecordFileListItem(int i, int page, char fileName[]) {
    settextstyle(26, 0, "微软雅黑");
    setbkmode(TRANSPARENT);
    char str[20];
    char str2[20];
    itoa(i + 1, str, 10);

    itoa(page + 1, str2, 10);
    strcat(str2, "-");
    outtextxy(40, 120 + 25 * (i - 15 * page), strcat(str2, str));//绘制序号
    outtextxy(120, 120 + 25 * (i - 15 * page), fileName);//绘制得分

    return 1;
}
/*绘制按钮*/
int drawReviewPageBtn() {
    setfillcolor(GREEN30);//设置填充颜色（RGB）
    setlinecolor(GREEN30);//设置线条颜色（RGB）
    fillroundrect(170, 550, 290, 590, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
    settextcolor(GREEN40);//设置文字颜色（RGB）                      
    settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
    setbkmode(TRANSPARENT);//设置背景模式（透明）                   
    outtextxy(208, 554, "返回");;//输出文字（X，Y，文字）         
    setlinecolor(GREEN10);//设置线条颜色（RGB）



    setfillcolor(GREEN30);//设置填充颜色（RGB）
    setlinecolor(GREEN30);//设置线条颜色（RGB）
    fillroundrect(30, 550, 150, 590, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
    settextcolor(GREEN40);//设置文字颜色（RGB）                      
    settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
    setbkmode(TRANSPARENT);//设置背景模式（透明）                   
    outtextxy(56, 554, "上一页");;//输出文字（X，Y，文字）         
    setlinecolor(GREEN10);//设置线条颜色（RGB）



    setfillcolor(GREEN30);//设置填充颜色（RGB）
    setlinecolor(GREEN30);//设置线条颜色（RGB）
    fillroundrect(310, 550, 430, 590, 40, 40);//圆角（左端，顶部，右端，底部，圆滑度，圆滑度）
    settextcolor(GREEN40);//设置文字颜色（RGB）                      
    settextstyle(30, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）            
    setbkmode(TRANSPARENT);//设置背景模式（透明）                   
    outtextxy(336, 554, "下一页");;//输出文字（X，Y，文字）         
    setlinecolor(GREEN10);//设置线条颜色（RGB）
    return 1;
}
/*NEW:棋局再现_文件列表*/
int drawRecordFileList() {
    windowInit(REVIEWPAGE);
    char fileNameList[200][70] = {};
    int page = 0;
    int item = 0;
    int res;
    if (!access("record", 0)) {
    }
    else {
        res = mkdir("record");// 返回 0 表示创建成功，-1 表示失败
        if (res == 0) {
        }
        else {
            creatMessage(ERRORCREATFOLDER);
        }
    }
    DIR* dir = opendir("./record");
    //成功：返回指向该目录的结构体目录
    //失败：返回NULL
    if (dir == NULL)
    {
        //printf("打开失败！\n");
        creatMessage(ERROROPENFOLDER);
    }
    //定义一个目录结构体题指针
    struct dirent* dirp;
    int i = 0;
    while (1)
    {
        dirp = readdir(dir);
        //readdir打开目录，返回值为一个结构体
        if (dirp == NULL)
        {
            break;
        }
        //dirp->d_type 是这个指针指向文件的类型
        //DT_DIR  目录
        //DT_REG  文件
        if (dirp->d_type == DT_DIR)
        {
        }
        else if (dirp->d_type == DT_REG)
        {
            strcpy(fileNameList[i++], dirp->d_name);
        }
        else
        {
            break;
        }

    }
    //关闭目录
    closedir(dir);
    //printf("\n\n");
    drawReviewPageBtn();
    int fileNameListLen = 0;
    for (int k = 0; 0 != strcmp(fileNameList[k], ""); k++) {
        //printf("%s\n",fileNameList[k]);

        fileNameListLen = k;
    }
    char str3[100] = "record/";
    while (1) {

        for (int j = 15 * page; j < 15 * (page + 1) && 0 != strcmp(fileNameList[j], ""); j++) {
            drawRecordFileListItem(j, page, fileNameList[j]);
        }
        if (MouseHit()) {//点击鼠标操作
            MOUSEMSG msg = GetMouseMsg();//定义
            switch (msg.uMsg) {//结构体
            case WM_LBUTTONDOWN://点击左键
                switch (getMouseClickBtn(msg.x, msg.y, REVIEWPAGE)) {
                case 0:item = 0; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 1:item = 1; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 2:item = 2; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 3:item = 3; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 4:item = 4; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 5:item = 5; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 6:item = 6; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 7:item = 7; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 8:item = 8; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 9:item = 9; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 10:item = 10; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 11:item = 11; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 12:item = 12; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 13:item = 13; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case 14:item = 14; recoveryByPlayRecord(strcat(str3, fileNameList[15 * page + item])); break;
                case -2: {
                    if (page + 1 > 1. * fileNameListLen / 15) {
                        //printf("ENDOFLIST\n");
                        creatMessage(EOL);
                    }
                    else {
                        page++;
                        cleardevice();
                        setbkcolor(bkColor);//设置背景颜色
                        cleardevice();//清屏
                        settextcolor(GREEN40);//设置文字颜色
                        settextstyle(38, 0, "微软雅黑");//设置文字样式
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
                        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
                        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）
                        drawReviewPageBtn();
                    }
                    break;
                }
                case -3: {

                    if (page - 1 < 0) {
                        creatMessage(HOL);
                        //printf("HEADOFLIST\n");
                    }
                    else {
                        page--;
                        cleardevice();
                        setbkcolor(bkColor);//设置背景颜色
                        cleardevice();//清屏
                        settextcolor(GREEN40);//设置文字颜色
                        settextstyle(38, 0, "微软雅黑");//设置文字样式
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 20, "棋局再现");//输出文字（X位置，Y位置，文字）
                        settextstyle(24, 0, "微软雅黑");//设置文字样式（高度，间隔，字体）
                        setbkmode(TRANSPARENT);//设置背景模式（透明）
                        outtextxy(40, 60, "记录您保存的棋局，可在“record”文件夹下查");//输出文字（X，Y，文字）
                        outtextxy(40, 84, "看和更改棋局文件");//输出文字（X，Y，文字）
                        drawReviewPageBtn();
                    }
                    break;
                }
                }

            }
        }
    }
    return 1;
}
/*测试*/
int testFun() {
    return 1;
}
/*主函数*/
int main() {
    testFun();//运行测试
    dataInit();//初始化数据
    initPage();//创建界面
    closegraph();//关闭界面
    return 0;
}
```
## 继续开发
如何在你的 Windows 电脑上阅览并完善我的代码
### 1. 下载并安装 Visual Studio 下载器的安装器
https://visualstudio.microsoft.com/zh-hans/
选择 Community 版本填写信息并下载安装
### 2. 通过 Visual Studio 下载器下载并安装语言环境
勾选 使用 C++ 的桌面开发
并在右侧详细的可选组件信息里面全部勾选
勾选完成后保持网络连接，等候其完成下载和安装
### 3. 在 Visual Studio 中继续
1. 下载必要的压缩包.zip文件
https://www.kdocs.cn/view/l/ckcMwcaeK5Zt?openfrom=docs
2. 解压缩CHomework_ZJUT.zip文件
3. 在 VisualStudio 中打开.sln文件即可
### 4. 导入 EasyX 库
1. 双击EasyX_2023大暑版.exe
2. 点击“下一步”
3. 点击“安装”
4. 在.cpp文件中引用
#include <graphics.h>
### 5. 添加打包组件
点击扩展-管理扩展
搜索“install”，检索出来的第一项选中安装
### 6. 打包生成安装引导
1. 在解决方案管理器中右键“Setup”
2. 点击“生成”或“重新生成”或“安装”
### 7. 配置打包资源
1. 若添加了资源（如图片资源，图标资源等）需要在Setup里面右键“Application Folder”-Add来添加💡打包资源中
> 💡更多关于打包的操作还可参考打包与安装
### 8. EasyX库基本函数简介
1. 视频课程
> 💡EasyX必须添加头文件在.cpp文件中，在.c文件中添加会报错
2. EasyX帮助文档
3. 窗口函数
```C
initgraph(640,480);                //创建一个640*480大小的窗口
initgraph(640, 480, EX_SHOWCONSOLE);    //创建一个尺寸为 640x480 的绘图窗口，同时显示控制台窗口，这个最后一个参数有多个选择，可以在 VS 中右键-转到定义来查看其他的参数

#define SHOWCONSOLE       1        // Maintain the console window when creating a graphics window
#define NOCLOSE           2        // Disable the close button
#define NOMINIMIZE        4        // Disable the minimize button
#define EW_SHOWCONSOLE    1        // Maintain the console window when creating a graphics window
#define EW_NOCLOSE        2        // Disable the close button
#define EW_NOMINIMIZE     4        // Disable the minimize button
#define EW_DBLCLKS        8        // Support double-click events

setbkcolor(bkColor);               //设置窗口背景颜色                    
cleardevice();                     //清空绘图设备

closegraph();                      //关闭窗口4. 绘图函数
```
所有绘制前应规范设置画笔颜色、粗细等样式；填充颜色等
```C
setfillcolor(BLUE);                //设置填充颜色
setlinecolor(BLUE);                //设置线条颜色
setlinestyle(PS_SOLID, 10);        //设置划线为宽度为10的实线
```
> 💡这里可以添加更多的参数来实现不同的线条样式可访问官方文档EasyX 文档 - setlinestyle
> 更多图形绘制函数参见https://docs.easyx.cn/zh-cn/drawing-func
5. 绘制文字函数
```C
settextcolor(GREEN40);                //设置字体颜色        
settextstyle(38, 0, "微软雅黑");       //设置字体大小，自适应宽度，字体
setbkmode(TRANSPARENT);               //设置字体背景透明    
outtextxy(40, 20, "关于");            //在（40，20）坐标位置写“关于”💡更多绘制文本函数参加EasyX 文档 - 文字输出相关函数
```
6. 鼠标操作
7. 键盘操作
### 9. 文件类操作
1. 视频学习
2. 读取文件
3. 写入文件
