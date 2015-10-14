title: 三维重建之摄像机标定：DLT方法
tags:
  - DLT
id: 359
categories:
  - 三维重建
date: 2014-05-31 20:37:01
---

上周写了摄像机标定的原理，这次主要来谈谈几种摄像机标定的方法。

一、直接线性变换（DLT变换）

直接线性是将像点和特点的成像几何关系在齐次坐标下写成透视投影矩阵的形式：

[![DLT0](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT0.png)](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT0.png)

<!--more-->其中(u,v,1)为图像坐标系下的点的齐次坐标，(Xw, Yw, Zw)为世界坐标系下的空间点的欧氏坐标，P为3×4的透视投影矩阵，S为未知尺度因子。也就是说，我们把相机的内参以及旋转平衡当成一个整体P，直接对P进行求解。消去s，可以得到方程组：

[![DLT](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT.png)](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT.png)

当已知N个空间点和对应的图像上的点时，可以得到一个含有2*N个方程的方程组：

[![矩阵](http://www.legendtkl.com/wp-content/uploads/2014/05/矩阵.png)](http://www.legendtkl.com/wp-content/uploads/2014/05/矩阵.png)

其中A为2N*12的矩阵，每一对对应点对应两行矩阵如下：

[![DLT2](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT2.png)](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT2.png)

L为透视投影矩阵元素组成的向量为[p11,p12,p13,p14,p21,p22,p23,p24,p31,p32,p33,p34]的转置（wordpress编辑器无力吐糟了...）。相机定标的任务就是寻找合适的L，使得||AL||最小.12个未知数，12个线性方程，也就是6对对应点就可以解出来了。但是这么解出来的解不是唯一。又由旋转矩阵的正交性和Z方向上的平移量不为0，可以引入约束

[![DLT3](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT3.png)](http://www.legendtkl.com/wp-content/uploads/2014/05/DLT3.png)

L'为L的前11个元素组成的向量,C为A前11列组成的矩阵，B为A第12列组成的向量。求解的过程通过构造伪逆矩阵来求解。其实很简单，我用代码简单实现如下，其中Utilities::matSet2D(cv::Mat M, int x, int y, double val)完成设置M矩阵的位置(y,x)设置为val的操作。
<pre>[cce_cpp tab_size="4"]
void calDLTParams(std::string objCornersFile,std::string imgCornersFile)
{
	cv::vector&lt;cv::Point3f&gt; objCorner;
	cv::vector&lt;cv::Point2f&gt; imgCorner; 

	std::ifstream in(objCornersFile.c_str());
	if(!in){
		std::cout &lt;&lt; "Load objCorners ERROR: can't open file " &lt;&lt; objCornersFile &lt;&lt; std::endl;
		return;
	}
	while(in){
		cv::Point3f tmp;
		in &gt;&gt; tmp.x &gt;&gt; tmp.y &gt;&gt; tmp.z;
		objCorner.push_back(tmp);
	}
	in.close();

	in.open(imgCornersFile.c_str());
	if(!in){
		std::cout &lt;&lt; "Load imgCorners ERROR: can't open file " &lt;&lt; imgCornersFile &lt;&lt; std::endl;
		return;
	}
	while(in){
		cv::Point2f tmp;
		in &gt;&gt; tmp.x &gt;&gt; tmp.y;
		imgCorner.push_back(tmp);
	}
	in.close();

	int N = imgCorner.size();
	cv::Mat C(2*N, 11, CV_64F);
	cv::Mat B(2*N, 1, CV_64F);

	for(int i=0; i&lt;2*N; i=i+2)
	{
		Utilities::matSet2D(C, 0, i, objCorner[i/2].x);
		Utilities::matSet2D(C, 1, i, objCorner[i/2].y);
		Utilities::matSet2D(C, 2, i, objCorner[i/2].z);
		Utilities::matSet2D(C, 3, i, 1);

		for(int j=4; j&lt;8; j++)
			Utilities::matSet2D(C, j, i, 0);

		Utilities::matSet2D(C, 8, i, objCorner[i/2].x*imgCorner[i/2].x);
		Utilities::matSet2D(C, 9, i, objCorner[i/2].y*imgCorner[i/2].x);
		Utilities::matSet2D(C, 10, i, objCorner[i/2].z*imgCorner[i/2].x);
	}
	for(int i=1; i&lt;2*N; i=i+2)
	{
		for(int j=0; j&lt;4; j++)
			Utilities::matSet2D(C, j, i, 0);

		Utilities::matSet2D(C, 4, i, objCorner[i/2].x);
		Utilities::matSet2D(C, 5, i, objCorner[i/2].y);
		Utilities::matSet2D(C, 6, i, objCorner[i/2].z);
		Utilities::matSet2D(C, 7, i, 1);

		Utilities::matSet2D(C, 8, i, objCorner[i/2].x*imgCorner[i/2].y);
		Utilities::matSet2D(C, 9, i, objCorner[i/2].y*imgCorner[i/2].y);
		Utilities::matSet2D(C, 10, i, objCorner[i/2].z*imgCorner[i/2].y);
	}

	for(int i=0; i&lt;2*N; i=i+2)
		Utilities::matSet2D(B, 0, i, imgCorner[i/2].x);
	for(int i=1; i&lt;2*N; i=i+2)
		Utilities::matSet2D(B, 0, i, imgCorner[i/2].y);

	cv::Mat L = -(C.t()*C).inv()*C.t()*B;
	std::string folderPath="";
	folderPath += "/DLT.txt";

	std::ofstream out(folderPath.c_str());
	if(!out){
		std::cout &lt;&lt; "DLT write ERROR: can't open file " &lt;&lt; folderPath &lt;&lt; std::endl;
		return;
	}
	for(int i=0; i&lt;11; i++)
		out &lt;&lt; Utilities::matGet2D(L, 0, i) &lt;&lt; '\n';
	out.flush();
}
[/cce_cpp]</pre>
又到假期了，大家双节愉快！

&nbsp;

&nbsp;