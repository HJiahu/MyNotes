Notes for opencv .
OpenCV2 cooking book

### ��һ�£�  2015-09-06 07:57:40
introduce opencv2.0
opencvʹ�õ����ֿռ���cv�����������ʾimage�ĺ������﷨�����ǣ�cv::imshow()
```
int main( int argc, char** argv ) 
{
	cv::Mat img;
	img = cv::imread("../Imags/lena.jpg",1);
	//��opencv��C�ӿ���ʹ�������������������ͼ��Ipl��ʾһ��intel�Ŀ�����P29
	// IplImage* iplImage = cvLoadImage("c:\\img.jpg");     
	//���Ժܷ���Ľ�iplImageָ���C �ṹ��ת��ΪMat�ࣺ
	//cv::Mat image4(iplImage,false);
	//cvReleaseImage(&iplImage);//����ʹ����ȷ����������IplImage�ṹ
	cv::namedWindow("Image_1",cv::WINDOW_AUTOSIZE);
	while (1) {
		cv::imshow("Image_1", img);  //��ʽ���ںͱ���ͼƬ���ڴ���������ġ�
		cv::waitKey(0);
		cv::flip(img, img, 1);
	}
}
```
-	cv::Mat img //  Mat is a class ��
> Mat ʹ�������ü�����ǳ���ƣ�Ϊ��ʵ����ƣ�ʹ�÷���img.copyTo(cv::Mat img_1)             
> img.size().height                
> img.size().width             
> ***img.data***  ��ָ��ͼ��洢�ռ��ָ�룬ʹ������������Բ���ͼƬ�Ƿ���ȷ��ȡ��          
> we can creat matrix data by Mat :         
> `cv::Mat img(240,320,CV_8U,cv::Scalar(100));`                   
> Ϊ��ʵ��ǳż�ϣ���ΪMat���ǳ�������⣬��ò�Ҫ�����з���һ��Mat�࣬�����Ͷ�ʱ�����ɶ���֮���һЩӰ�죬���������ĸ��Ӷȣ�
> ```
> class Test{
> public:
>	 Mat GetImg(){return img;}   //��ò�Ҫֱ�ӷ������е�Mat�࣬����ʹ��������������Mat.copyTo(Mat dst);
> private:
> 	Mat img;
> }
> ```

-	cv::namedWindow("Original Image"); // define the window
-	cv::imshow("Original Image", image); // show the image
-	Mat imread( const string& filename, int flags=1 ); 
-	cv::imwrite(filename , Mat,...)
-	cv::flip(Mat src ,Mat dst,int flipcode)    //if flipcode == 0 vertical if flipcode == 1 horizontal 
-	cv::waitKey(int delay = 0) //default ,this fun will wait for ever if no key is pressed ,if delay is not 0... 

### �ڶ���
-	����һ���Ҷ�ͼ���ԣ�ÿһ��Ԫ�ش���һ�����صĻҶ�ֵ������0��ʾ��ɫ��255��ʾ��ɫ��
> ����cv::Mat�Ĺ��캯�������ǿ��Դ�����ͬ�Ĺ��캯����������ͬ�ĵ�ͼ����Ҷ�ͷ����ɫͼ...
```
	#include <iostream>
	#include <cstdlib>
	#include <ctime>
	#include <opencv2/highgui/highgui.hpp>
	void salt_image(cv::Mat  &img , int n)
	{
		srand(time(NULL));
		for (int i = 0; i < n ; i++) {
			int row_num = rand()%img.rows ; 
			int col_num = rand()%img.cols ; 
			if (img.channels() == 1) {
				img.at<uchar>(row_num , col_num) = 255;
				//cv::Mat_ img_2 = img ; //shallow copy 
				//img(row_num , col_num) = 255;
			}
			else {
				if(img.channels() == 3){
					img.at<cv::Vec3b>(row_num , col_num)[0] = 255;
					img.at<cv::Vec3b>(row_num , col_num)[1] = 255;
					img.at<cv::Vec3b>(row_num , col_num)[2] = 255;
				}
			}
		}		
	}
```

-	cv::Mat::at<typename>(int i , int j) can over load 
> ʹ��CV::Mat_<typename >����Լ�ĳЩ������������Mat_������������� () ��cv::Mat_::operator()(int i , int j);��cv::Mat::at()����ͬ��˼��

-	��opencv�У���ɫ����ͨ��ͼƬ������������ͨ����˳���ǣ�BGR��blue��ɫ�ڵ�һ���ֽڡ�
-	***��Ϊ��Ч�����⣬ͼƬ���ڴ��д洢ʱ���е�������������ͼ���ʵ���е���������ͬ��һ�����ڴ������ݶ�����������ݵĴ����ٶȡ��������ǲ�����Ϊͼ��Ĵ洢�������ġ�***
> ��Mat���У�rows���Ը���ͼ�����ʵ������cols������ʵ��������ô��cols���ǲ�����ϵͳΪ��Ч�ʶ�����ӵ����ء�step��������ÿ�е��ֽ�����elemSize����ÿ�����ص��ֽ�����total()����ͼƬ������������







page 26
### �����£�











### some functions structure etc for opencv 
#### functions 
- Mat imread( const string &filename, int flags=1 );
	> return a structure(cv::Mat) to handle images .
	flags Specifies color type of the loaded image:
	>0 the loaded image is forced to be a 3-channel color image
	=0 the loaded image is forced to be grayscale
	<0 the loaded image will be loaded as-is (note that in the current
	implementation the alpha channel, if any, is stripped from the output
	image, e.g. 4-channel RGBA image will be loaded as RGB if f lags �� 0).

- bool Mat::empty() const;  
	> check to see if an image was in fact read.

- void cv::namedWindow( const string& winname, int flags ); 
	> winname is the name of new window and Future HighGUI calls that interact
	with this window will refer to it by this name.
	> flag can be 0(the default value) and cv::WINDOW_AUTOSIZE.
	if 0:all images will display in the same size .
	if auto differdnt pictures will show in different size .

- void imshow( const string& winname, const Mat& image );
	> winname Name of the window.
	image Image to be shown.

- int waitKey(int delay=0); 
	> this fun is used to wait a keypress event.if delay if >=0 this functions 
	will wait for infinit .if delay is >0 ,this functions will return if some 
	key is pressed or wait time ivvs more than delay.
	Returns the code of the pressed key or -1 if no key was pressed before the 
	specified time had elapsed.
	*	Note: This function is the only method in HighGUI that can fetch and handle events, so it
	needs to be called periodically for normal event processing, unless HighGUI is used within some
	environment that takes care of event processing.

- void DestroyWindow( const char* name );
	> name is the window's name .




	## structures && classes

	### class :
- cv::Mat:         opencv use this structure to handle all kinds of images .
- cv::VideoCapture Class for video capturing from video files or cameras. 

	### structure :





