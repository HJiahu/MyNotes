����ժ�ԣ�OpenCV2 cooking book
-	Ŀ¼ <span id="Index"/> 
	-	[��һ�� ���](#1)           
	-	[�ڶ��� ���صĲ���](#2)          
	-	[������ ����������ͼƬ](#3)
	-	[������ ��ֱ��ͼ����������](#4)



### ��һ�£�  2015-09-06 07:57:40 <h1 id="1"></h1> 
introduce opencv2.0
opencvʹ�õ����ֿռ���cv�����������ʾimage�ĺ������﷨�����ǣ�cv::imshow() 
> //��opencv��C�ӿ���ʹ�������������������ͼ��Ipl��ʾһ��intel�Ŀ�����P29                
// IplImage* iplImage = cvLoadImage("c:\\img.jpg");             
//���Ժܷ���Ľ�iplImageָ���C �ṹ��ת��ΪMat�ࣺ               
//cv::Mat image4(iplImage,false);//false ��ʾ��ԭ��������ͼƬ����

-	cv::Mat img //  Mat is a class ��
> Mat ʹ�������ü�����ǳ���ƣ�Ϊ��ʵ����ƣ�ʹ�÷���`void img.copyTo(cv::Mat img_1) const;`������ȿ�������`cv::Mat Mat::clone() const;`         
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

-	cv::Mat_<> matNmae;//cv::Mat_ ��һ�����ģ�壬����֪ͼ��ĸ�ʽ��ʱ��ʹ�����ģ����Լ�һЩ��������Ϊ��Mat_ �ж�����һЩMat����û�еĲ��������ص��������
> cv::Mat_<uchar> img;//����ʹ��img(x ,  y)�������ؽ��и�ֵ��

-	cv::namedWindow("Original Image"); // define the window
-	cv::imshow("Original Image", image); // show the image
-	Mat imread( const string& filename, int flags=1 ); 
-	cv::imwrite(filename , Mat,...)
-	cv::flip(Mat src ,Mat dst,int flipcode)    //if flipcode == 0 vertical if `flipcode > 0` horizontal ,if `flipcode < 0` ,both
-	cv::waitKey(int delay = 0) //default ,this fun will wait for ever if no key is pressed ,if delay is not 0...          
-	cv::Mat::reshape(...)  ��ͼ��ά�Ⱥ����������ĸ��ġ�

### �ڶ��� ���صĲ��� [\[Ŀ¼\]](#Index) <span id="2"/> 
-	some functions in this capture
```
cv::Mat::at<cv::Vec3b>(x, y)[];//���ص���һ����ֵ
cv::Mat_<uchar> img;//������cv::Mat ��û�е�һЩ��Ա����
cv::Mat::isContinues();//�Ƿ���padding
cv::Mat::step;//����ÿ�е��ֽ���������padding�������������صĸ���
uchar cv::Mat::data;//��������ͼƬ���ڴ�ĵ�һ���ֽڵ�ַ��ע�ⷵ�ص�ָ������Ϊuchar *
cv::elemSize();//����ÿ�����ص��ֽ���
cv::Mat::cols;//������ֵ
cv::Mat::rows;
uchar *cv::Mat::ptr<uchar>(y);//���ص�y����Ԫ��ָ��
int cv::Mat::channels();
cv::Mat cv::clone(void);//������ȸ��Ƶĸ���
cv::Mat::create(rows , cols , img.type());//�����ǰmat�����е����ݺ�create�еĲ�������ͬ�ģ���create�����κεĲ�����***creat����������ͼ����continue�ġ�***
itreator<???> cv::MatItreator_<> it;
itreator<> cv::Mat_<>::itreator it;//���������������ж�Ӧ�ĳ����͵�������
cv::Mat::begin<>() ;//
cv::Mat::end<>();//������Mat�е�begin��end����������ģ�壬��ʹ��cv::Mat_ʱ�Ϳ��Բ�ʹ��ָ�����͡�
cv::getTickCount();
cv::getTickFrequency();
static_cast<double> expression;//ǿ������ת����

```

-	����һ���Ҷ�ͼ���ԣ�ÿһ��Ԫ�ش���һ�����صĻҶ�ֵ������0��ʾ��ɫ��255��ʾ��ɫ��
> ����cv::Mat�Ĺ��캯�������ǿ����ò�ͬ�Ĺ��캯����������ͬ�ĵ�ͼ����Ҷ�ͼ����ɫͼ...

-	srand()��rand()��ͷ�ļ�cstdlib�У�time()��ͷ�ļ�ctime�С�srand(time(NULL))

-	`cv::Mat::at<typename>(int i , int j)`   //Ч�ʽϲ�
-	`cv::Mat::at<uchar>(x , y)`
-	`cv::Mat::at<cv::Vec3b>(x , y)[]`
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
				*data++= *data&mask + div/2;//&�����ȼ��ڹ�ϵ�����֮���߼������֮ǰ
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
	planes[0]+= image2;//add image2 to the blue channel of image1

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

### ������ ����������ͼƬ  <span id="3"/> [\[Ŀ¼\]](#Index)  
-	City block distancee �����⳵���룬�����پ��롣�����˵��������롱
-	Enclidean norm ��ŷ����¾��룬����֮��ľ��롣
	-	`cv::norm<int , 3>(...);`
-	***���ڿ����Ѿ����ڵ��������������ʱ��Ҫע�����ǵ���Ϊ�������������Vec3u�͵�����a��b��c=a-b�м��� - ���Ѿ�������saturate_cast()������������������Ҫע�⡣***

***���ģʽ***
1. �������ģʽ��strategy pattern�����򵥵�˵���ǽ�������װ�����С�
2. ��̬ģʽ��
3. MVCģʽ��modle-view-controler        GUI P84

**��ɫ�ռ��ת��**
-	The Structure and Properties of Color Spaces and the Representation of Color Images  :a useful book
-	BGR ��BGR  is not a perceptually uniform color space .
-	***CIE L*a*b* ��ɫ�ռ���һ���������۶��Ե�������ɫ�ռ䡣***
	-	L:0~100    a,b:-127 ~ +127
-	cv::cvtColor(tmp, tmp, CV_BGR2Lab);//����ת����ɫ�ռ䡣
	-	cv::cvtColor(color, gray, CV_BGR2Gray);
	-	CV_BGR2YCrCb


### ������ ��ֱ��ͼ����������  <span id="4"/> [\[Ŀ¼\]](#Index) 
-	`void calcHist( const Mat* arrays, int narrays, const int* channels, const Mat& mask, MatND& hist, int dims, const int* histSize, const float** ranges, bool uniform=true, bool accumulate=false );`
> 

-	`void calcHist( const Mat* arrays, int narrays, const int* channels, const Mat& mask, SparseMat& hist, int dims, const int* histSize, const float** ranges, bool uniform=true, bool accumulate=false );`

-	`cv::threshold(cv::Mat sor_img,cv::Mat threshold_img,60,255,cv::THRESH_BINARY)`a




 
