# C++ 日志记录模块

该模块从实际项目中产生，通过extern声明的方式，可在代码不同模块中生成日志，日志文件名称为随机码加用户指定名称，采用随机码是为了避免日志文件可能被覆盖的问题。

愿意的话你也能自己构建个人的日志记录模块，本次分享的模块实现方法比较简单，可能有些地方没考虑清楚。

### 源码：

```c++
//
// Created by jerry on 2/12/16.
//

#include <iostream>
#include <string>
#include <fstream>
#include <sys/time.h>
#include <unistd.h>
#include <stdlib.h>

namespace lg{
	class Log {
	private:
	    std::string m_log_file_path;
	    std::ofstream m_log_file;
	    bool m_cout_flag;
	public:
	    Log(const std::string file_path = "run.log", const bool cout_flag = true);
	    ~Log();

	    void close();
	    template<class T>
	    Log & operator << (T &log_data)
	    {
	    	m_log_file << log_data;
	    	m_log_file << std::flush;
#if ((defined _RM_DEC_PRINT) && (defined _RM_DEC_LOG_PRINT))
	    	if(m_cout_flag)
	    		std::cout << log_data << std::flush;    		
#endif	    
	    	return *this;
	    }
	};

	extern Log run_log;
}
```

```c++
//
// Created by jerry on 2/12/16.
//

#include "Log.h"

namespace lg{

	Log run_log;
    
	Log::Log(const std::string file_path, const bool cout_flag)
	{
		m_cout_flag = cout_flag;
		// system("mkdir $HOME/data/log");
        
	    struct timeval tv;     
	    gettimeofday(&tv,NULL);
	    std::random_device rd;  
	    std::default_random_engine e(rd());  
	    std::uniform_int_distribution<> u(0,1000000);
	    usleep(u(e));
        
        std::string home = getenv("HOME"); 
	    
        std::string log_file_path = home + "/data/log/" +
	                            std::to_string((tv.tv_sec * 1000) % 1000000 + tv.tv_usec / 1000) +
	                            std::to_string(u(e)) +
	 							file_path;
	    m_log_file_path = log_file_path;
	    m_log_file.open(m_log_file_path, std::ios::app);
	}

	Log::~Log()
	{
	    m_log_file.close();
	    std::cout << "-- Log Destructor successfully!" << std::endl;
	}

	void Log::close()
	{
		m_log_file.close();
	}

}
```

### 例程：

下述log_information为用户需要记录的日志信息。

```c++
//file test1.cpp
#include "Log.h"
//...	your code here
//...	your code here
lg::run_log << /***log_information1 here***/ << "\n";
```

```c++
//file test2.cpp
#include "Log.h"
//...	your code here
//...	your code here
lg::run_log << /***log_information2 here***/ << "\n";
```

最终日志输出为：

```
#file (rand_code)run.log
log_information1
log_information2
```

