
>在Ubuntu下使用opencv处理视频流时，**由于相机帧率跟不上（相机模块在另外一个线程运行，且帧率太低），导致算法会处理一些相同的图像**，从而返回相同的结果，如果将结果返回给伺服机构，则可能导致伺服机构奔溃。

想到三种解决方法：
1.	用高帧率的相机，但是由于经费问题，此方案暂缓执行；
2.	判断返回值是否相同，如果返回的数据完全相同，则有比较高的置信度认为这是通过同一幅图像返回的结果；
3.	在处理前判断相机获取到的图像是否相同，因为在相机实时获取图像时，即使是相机不懂，获取的图像在像素层面上也不是完全相同的，从而我们在处理前可以判断相邻两副图像是否完全相同来确保给伺服机构的数据是正常的数据;


第三种方案伪代码如下：
``` C++
if (!m_frame_store.empty())	//上一次图像不为空
    {
        long long int diff_counts = 0;   //不同像素统计值
        int frame_element_size = m_frame_store.total();  //像素总数
        const int diff_counts_upper = 10;  //不同值上限，超过则认为是不同图像
        for (int i = 0; i < frame_element_size; i++)
        {
            if (m_frame_store.data[i] != frame.data[i])
            {
                diff_counts++;
                if (diff_counts > diff_counts_upper)  //满足不同图像特征，则停止遍历
                    break;
            }
        }
        if (diff_counts > diff_counts_upper)
        {
            frame.copyTo(m_frame_store);
            m_same_frame_count = 0;
            return 1;
        }
        else   //不满足不同图像特征
        {
            m_same_frame_count++;  //迭代次数达到上限则认为相机掉线，缓存区未更新
            if (m_same_frame_count > m_same_frame_count_upper)
                restartCamera();
            return 2;
        }
    }
    else
    {
        frame.copyTo(m_frame_store);
        m_same_frame_count = 0;
        return 1;
    }
```

