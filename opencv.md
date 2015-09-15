����ժ�ԣ�OpenCV2 cooking book

### ��һ�£�  2015-09-06 07:57:40
introduce opencv2.0
opencvʹ�õ����ֿռ���cv�����������ʾimage�ĺ������﷨�����ǣ�cv::imshow() 
> //��opencv��C�ӿ���ʹ�������������������ͼ��Ipl��ʾһ��intel�Ŀ�����P29                
// IplImage* iplImage = cvLoadImage("c:\\img.jpg");             
//���Ժܷ���Ľ�iplImageָ���C �ṹ��ת��ΪMat�ࣺ               
//cv::Mat image4(iplImage,false);

-	cv::Mat img //  Mat is a class ��
> Mat ʹ�������ü�����ǳ���ƣ�Ϊ��ʵ����ƣ�ʹ�÷���img.copyTo(cv::Mat img_1)������ȿ�������cv::Mat Mat::clone() .         
> img.size().height                
> img.size().width             
> ***img.data***  ��ָ��ͼ��洢�ռ��ָ�룬ʹ������������Բ���ͼƬ�Ƿ���ȷ���롣          
> we can create matrix data by Mat :         
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
-	cv::Mat::reshape(...)  ��ͼ��ά�Ⱥ����������ĸ��ġ�

### �ڶ���
-	����һ���Ҷ�ͼ���ԣ�ÿһ��Ԫ�ش���һ�����صĻҶ�ֵ������0��ʾ��ɫ��255��ʾ��ɫ��
> ����cv::Mat�Ĺ��캯�������ǿ����ò�ͬ�Ĺ��캯����������ͬ�ĵ�ͼ����Ҷ�ͼ����ɫͼ...

-	srand()��rand()��ͷ�ļ�cstdlib�У�time()��ͷ�ļ�ctime�С�srand(time(NULL))

-	`cv::Mat::at<typename>(int i , int j)`   //Ч�ʽϲ�
> ʹ��`cv::Mat_<typename >`����Լ�ĳЩ������������`Mat_`������������� () ��`cv::Mat_::operator()(int i , int j);`��`cv::Mat::at()`����ͬ��˼��

-	`uchar * data Mat::ptr<typename>(int i)`   //����ͼƬ��i�е��ڴ��׵�ַ��
-	��opencv�У���ɫ����ͨ��ͼƬ������������ͨ����˳���ǣ�***BGR***��blue��ɫ�ڵ�һ���ֽڡ�
-	***��ΪЧ�����⣨�ڴ���룬�������ݵĴ����ٶȣ���ͼƬ���ڴ��д洢ʱ���е�������������ͼ���ʵ���е���������ͬ��һ�����ڴ������ݶ�����������ݵĴ����ٶȡ��������ǲ�����Ϊͼ��Ĵ洢�������ġ�***
> ��mat���У�***isContinuous()***����������ͼ�����ڴ����Ƿ������洢�����Ƿ���padding��rows���Ը���ͼ�����ʵ������cols������ʵ��������ô��cols���ǲ�����ϵͳΪ��Ч�ʶ�����ӵ����أ�������ӵ�����ʱ������ʽ�����ģ���step��������ÿ�е��ֽ���������padding����elemSize����ÿ�����ص��ֽ�����total()����ͼƬ������������

```
	void colorReduce(cv::Mat &image, int div=64) {
		int nl= image.rows; // number of lines
		int nc= image.cols ; // number of columns
		// is it a continous image
		if (image.isContinuous()) {
			// then no padded pixels
			nc= nc*nl;
			nl= 1; // it is now a 1D array
		}
		//div ��= 2^n  Լ���� 
		int n= static_cast<int>(log(static_cast<double>(div))/log(2.0));
		// mask used to round the pixel value
		uchar mask= 0xFF<<n; // e.g. for div=16, mask= 0xF0
		// for all pixels
		for (int j=0; j<nl; j++) {
			// pointer to first column of line j
			//�ڲ�����Ч�ʵ�����£����ǿ���ʹ�õ���������������   P49
			uchar* data= image.ptr<uchar>(j);
			for (int i=0; i<nc; i++) {
				// process each pixel ---------------------
				//p_row[j] = p_row[j]/n*n + n/2;//��������㲻�ᱻ�������Ż�������
				//p_row[j] = p_row[j] - p_row[j]%n +n/2;  this is slower
				//��������޶�n��ȡֵ��2������������ôЧ����ߵķ�����λ���㣺
				*data++= *data&mask + div/2;
				*data++= *data&mask + div/2;
				*data++= *data&mask + div/2;
				// end of pixel processing ----------------
			} // end of line
		}
	}

ʹ����������صİ汾��
image=(image&cv::Scalar(mask,mask,mask)) + cv::Scalar(div/2,div/2,div/2);

```
����ĺ�����ֱ����Դ�����Ͻ��в�����Ϊ�˲���Դͼ���в������ǿ��Դ���һ���µ�Mat�����磺
```
cv::Mat img = result;
result.creat(img.rows , img.cols, img.type());//�������������ͼ����û��padding�ģ�������Ч�����⡣�������result�Ѿ����˺Ͳ�����ͬ��ͼƬ�洢������ô���������ʲô��������
```
P49ҳ���������ʹ�õ��������������ء�

Ϊ�˲��Գ�������ܣ�opencv�ṩ��cv::getTickCount()��cv:getTickFrequency()����������ǰ�߻�ôӿ�����ʼ����ǰΪֹcpu��tick���������߾ͻ����cpu��ʱ��Ƶ�ʡ��ڲ��Գ���ǰ��ֱ�ʹ��getTickCount()�����һ��tick�������������Ϊ���tick�����ٳ���Ƶ�ʼ���ʱ�䡣

���ص��ٽ����ض�ȡ��2015-09-13 08:33:34          
-	�񻯲�����`sharpened_pixel= 5*current-left-right-up-down;` ����Ĳ����Ƕ�Դʹ������ָ�룬��Ŀ��ͼ��ʹ��һ��ָ�롣P56
-	`cv::saturate_cast<typename>(...) `�������ݵ���������� `cv::saturate_cast<uchar>( data )` ��data��ֵ������0��255��
-	cv::Mat::create(cv::Mat::size() , cv::Mat::type()) Ҫô����һ���µĴ洢������padding����Ҫô�Ͳ����κ����飨�Ѵ�������Ҫ��Ĵ洢������
-	cv::Mat::row(int n)::setTo(cv::Scalar(0 , 0 , 0));
-	***ʹ���Ѿ����ڵĺ����������񻯺�����***
> `cv::Mat kernel(3, 3, CV_32F, cv::Scalar(0)`            
> `kernel.at<float>(i , j) = ...`         
> `cv::filter2D(img , dst , img.depth() , kernel)`   

***ͼƬ�ĵ���***       
```
	// c[i]= a[i]+b[i];
	cv::add(imageA,imageB,resultC);
	// c[i]= a[i]+k;
	cv::add(imageA,cv::Scalar(k),resultC);
	// c[i]= k1*a[1]+k2*b[i]+k3;
	cv::addWeighted(imageA,k1,imageB,k2,k3,resultC);
	result= 0.7*image1+0.9*image2;  //ͼ�����������
	// c[i]= k*a[1]+b[i];
	cv::scaleAdd(imageA,k,imageB,resultC);
```


��opencv���кܶ�ֱ�Ӷ�ͼ�����ؽ��д���ĺ�����������Щ�����󲿷ݶ��ж�Ӧ����������غ����������ﲻһһ�г��������API �ֲᡣ

***�����ɫͼ��ͬ����ɫͨ��***
```
	// create vector of 3 images
	std::vector<cv::Mat> planes;
	// split 1 3-channel image into 3 1-channel images
	cv::split(image1,planes);
	// add to blue channel
	planes[0]+= image2;

	// merge the 3 1-channel images into 1 3-channel image
	cv::merge(planes,result);
```

***ROI***
> Region of interset     
> `cv::Mat roi_img = img(cv:Rect(x , y ,length ,height)`            
> `cv::Mat roi_img = img(cv::Rang(from , to) , cv::Rang(from ,to)`        
```
	// define ROI
	imageROI= image(cv::Rect(385,270,logo.cols,logo.rows));
	// load the mask (must be gray-level)
	cv::Mat mask= cv::imread("logo.bmp",0);
	// copy to ROI with mask
	logo.copyTo(imageROI,mask);
```

### ������ ����������ͼƬ
-	City block distancee �����⳵���룬�����پ��롣�����˵��������롱
-	Enclidean norm ��ŷ����¾��룬����֮��ľ��롣
	-	`cv::norm<int , 3>(...);`
-	***���ڿ����Ѿ����ڵ��������������ʱ��Ҫע�����ǵ���Ϊ�������������Vec3u�͵�����a��b��c=a-b�м��� - ���Ѿ�������saturate_cast()������������������Ҫע�⡣***




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





