title: 三维重建之摄像机标定：棋盘格标定
tags:
  - 棋盘格标定
id: 386
categories:
  - 三维重建
date: 2014-06-20 20:12:39
---

这次来讲棋盘格标定方法。方法很简单，用相机对棋盘格从不同角度拍多幅图像，然后计算摄像机参数。关于标定的原理和前面分析都一样，就是对应点对应约束方程，对参数矩阵进行求解。只不过对于棋盘格上的点假定它们的Z坐标都是0（一个平面上）。具体分析可以参考《Learning Opencv》那本书，中文翻译《学习OpenCV》，好在opencv已经将其集成进去了。下面主要写如何求摄像机内参。

<!--more-->本来以为很多，结果一梳理也太简单了。先寻找棋盘格角点，然后将角点的像素坐标和世界坐标传入函数calibrateCamera()函数就可以了，代码节选如下。
<pre>[cce_cpp tab_size="4"]
void CameraCalibration::findCorners(std::string filename, bool &amp;found, int X, int Y)
{
	std::stringstream imgfile;
	imgfile &lt;&lt; filename &lt;&lt; ".jpg";
	cv::Mat img = cv::imread(imgfile.str().c_str());
	cv::vector&lt;cv::Point2f&gt; cCam;

	cv::Mat img_copy;
	cv::Mat img_grey;
	img.copyTo(img_copy);

	cv::cvtColor(img, img_grey, CV_RGB2GRAY);
	img.copyTo(img_copy);

	found=cv::findChessboardCorners(img_grey, cvSize(X,Y), cCam, CV_CALIB_CB_ADAPTIVE_THRESH );

	if(!found)
		return;
	//继续找亚像素坐标
	if(found)
	{
		cv::cvtColor( img, img_grey, CV_RGB2GRAY );
		cv::cornerSubPix(img_grey, cCam, cvSize(20,20), cvSize(-1,-1), cvTermCriteria(CV_TERMCRIT_EPS+CV_TERMCRIT_ITER, 30, 0.1));
	}

void CameraCalibration::cameraCalibration()
{
	cv::vector&lt;cv::Mat&gt; camRotationVectors;
  	cv::vector&lt;cv::Mat&gt; camTranslationVectors;

	cv::calibrateCamera(ObjCorners, CamCorners, camImageSize, camMatrix, distortion, camRotationVectors,camTranslationVectors,0, 
		cv::TermCriteria( (cv::TermCriteria::COUNT)+(cv::TermCriteria::EPS), 30, DBL_EPSILON) );

	exportTxtFiles("matrix.txt", CAMCALIB_OUT_MATRIX);
	exportTxtFiles("distortion.txt", CAMCALIB_OUT_DISTORTION);
}
[/cce_cpp]</pre>
太简单了，算了，明天把外参的标定方法也写一下。