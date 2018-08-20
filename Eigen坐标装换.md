> （《视觉SLAM十四讲》第三讲习题7）设有小萝卜一号和二号在世界坐标系中。一号位姿q1 = [0.35, 0.2, 0.3, 0.1]，t1=[0.3, 0.1, 0.1]。二号位姿q2=[-0.5, 0.4, -0.1, 0.2], t2=[-0.1, 0.5, 0.3].某点在一号坐标系下坐标为p=[0.5, 0, 0.2].求p在二号坐标系下的坐标

假设在世界坐标系中p点的坐标为P。

用四元数做旋转则有（在Eigen中四元数旋转为q×v，数学中则为q×v×q^-1）：

- q1 × P + t1 = p1
- q2 × P + t2 = p2

由上两式分别解算出：

- P = q1^-1 × (p1 - t1)
- P = q2^-1 × (p2 - t2)

两式联立求解则得到：

p2 = q2 × q1^-1 × (p1 - t1) + t2

如果用欧拉矩阵（设一号欧拉矩阵为T1，二号欧拉矩阵为T2）则有：

- p1 = T1 × P
- p2 = T2 × P

求解P：

- P = T1^-1 × p1
- P = T2^-1 × p2

联立求解则有：

p2 = T2 × T1^-1 × p1



以下则是用Eigen实现的代码：

```C++
#include <iostream>

using namespace std;

#include <eigen3/Eigen/Core>
#include <eigen3/Eigen/Geometry>

int main()
{
    //四元数
    Eigen::Quaterniond q1 = Eigen::Quaterniond(0.35, 0.2, 0.3, 0.1).normalized();
    Eigen::Quaterniond q2 = Eigen::Quaterniond(-0.5, 0.4, -0.1, 0.2).normalized();
    //平移向量
    Eigen::Vector3d t1 = Eigen::Vector3d(0.3, 0.1, 0.1);
    Eigen::Vector3d t2 = Eigen::Vector3d(-0.1, 0.5, 0.3);
    //目标向量
    Eigen::Vector3d p1 = Eigen::Vector3d(0.5, 0, 0.2);
    Eigen::Vector3d p2;

    //打印输出
    // cout << q1.coeffs() << "\n"
    //      << q2.coeffs() << "\n"
    //      << t1.transpose() << "\n"
    //      << t2.transpose() << endl;

    //四元数求解
    p2 = q2 * q1.inverse() * (p1 - t1) + t2;
    cout << p2.transpose() << endl;

    //欧拉矩阵
    Eigen::Isometry3d T1 = Eigen::Isometry3d::Identity();
    Eigen::Isometry3d T2 = Eigen::Isometry3d::Identity();
    T1.rotate(q1.toRotationMatrix());
    T1.pretranslate(t1);
    T2.rotate(q2.toRotationMatrix());
    T2.pretranslate(t2);
    // cout << T1.matrix() << endl;
    // cout << T2.matrix() << endl;

    //欧拉矩阵求解
    p2 = T2 * T1.inverse() * p1;
    cout << p2.transpose() << endl;
}
```

