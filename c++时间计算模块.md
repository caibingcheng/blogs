# c++时间计算模块

可用于计算代码运行耗时、计算代码运行时间线（比如处理与运行时间相关函数）。

该模块从实际项目中产生，使用方式仁者见仁智者见智，设计思想可供参考。

### 源码：

```c++
//author: cai bingcheng, 2018

#pragma once

#include <iostream>
#include <chrono>

class GetCostTime
{
private:
    std::chrono::steady_clock::time_point m_time_pre;
    std::chrono::steady_clock::time_point m_time_now;
    std::chrono::steady_clock::time_point m_time_line;
    std::chrono::duration<double> m_time_cost;
    std::chrono::steady_clock::time_point m_time_run_begin;
    std::chrono::steady_clock::time_point m_time_run_end;
    std::chrono::duration<double> m_time_run_cost;
public:

    GetCostTime();
    ~GetCostTime();
    //init
    void GetCostTimeInit();
    //calculate cost time after init, refreshing after using
    double CalCostTime();
    //calculate cost time without refreshing
    double CalTimeLine();
    //init
    void RunTime();
    //calculate cost time after init without refreshing
    double GetRunTime();
    //sleep for m ms
    bool WaitTimeMs(double ms);
};

namespace tim{
    extern GetCostTime run_time;
}

```



```c++
//author: cai bingcheng, 2018

#include "GetCostTime.h"

using namespace std;

GetCostTime::GetCostTime()
{

}

GetCostTime::~GetCostTime()
{
  // std::cout << "-- GetCostTime Destructor successfully!" << std::endl;
}

void GetCostTime::GetCostTimeInit()
{
  m_time_now = std::chrono::steady_clock::now();
}

double GetCostTime::CalCostTime()
{
  m_time_pre = m_time_now;
  m_time_now = std::chrono::steady_clock::now();
  m_time_cost = std::chrono::duration_cast<chrono::duration<double>>(m_time_now - m_time_pre);
  return m_time_cost.count();
}

double GetCostTime::CalTimeLine()
{
  m_time_line = std::chrono::steady_clock::now();
  m_time_cost = std::chrono::duration_cast<chrono::duration<double>>(m_time_line - m_time_now);
  return m_time_cost.count();
}

void GetCostTime::RunTime()
{
  m_time_run_begin = std::chrono::steady_clock::now();
}

double GetCostTime::GetRunTime()
{
  m_time_run_end = std::chrono::steady_clock::now();
  m_time_run_cost = std::chrono::duration_cast<chrono::duration<double>>(m_time_run_end - m_time_run_begin);
  return m_time_run_cost.count();
}

//do not init while use this function
bool GetCostTime::WaitTimeMs(double ms)
{
  GetCostTimeInit();
  while(CalTimeLine() * 1000 < ms);
  return true;
}

namespace tim{
  GetCostTime run_time;
}

```

### 例程：

```c++
//CalCostTime
GetCostTime time;    
//init
time.GetCostTimeInit();
yourFunctions_1();
double cost_time = time.CalCostTime();
yourFunctions_2();
double cost_time = time.CalCostTime();
```

```c++
//GetRunTime
GetCostTime time;
time.RunTime();
yourFunctions();
double cost_time = time.GetRunTime();
```

上述两种方式有略微不同。

CalCostTime只需初始化一次，调用之后内部自动开始计时，适用于计算相邻代码块时间。

GetRunTime每次使用之前都需要初始化，适用于计算不相邻代码块时间。

```c++
//CalTimeLine
GetCostTime time;
time.GetCostTimeInit();
yourFunctions();
double cost_time = time.CalTimeLine();
if(cost_time > time_threshold)
    yourTasks();
```

CalTimeLine用于计算时间线，如果需要实现的功能与已运行时间有关，则可以使用该部分。