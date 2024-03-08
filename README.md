# StarImage_SimuIden
Star image simulation and identification

星图 模拟&识别 

软件功能：

1.星图模拟：可以输入姿态参数、相机参数模拟静态或动态星图，可以添加静态或动态的目标(假星)

2.星点提取：从图像中分割星点与背景，提取星点连通域，计算星点的质心坐标。

3.星图识别：识别图像中恒星，具体参见论文Graph Score Transfer

4.其他功能：导入图像、保存图像、镜像翻转图像、反投影星点、星点掩膜、目标探测。

面板介绍：

1. Attitude_Input 姿态输入
	Rand按钮：随机生成一组姿态参数，Ra赤经Dec赤纬对应的方向矢量服从在全天球概率均匀分布，Roll滚转角服从 ( -\pi, \pi ] 均匀分布。
	Ra Dec Roll：模拟星图的姿态参数，可以根据需要的参数手动输入。

2. Star_List_Generation 生成星表
	Get StarList按钮：按照"1."中姿态参数对应的光轴以及"4."中相机参数对应的视场，在星库中搜索视场内的恒星，并显示在位于左上的列表中。
	Catalog：可以选择生成星表、模拟星图、反投影时使用的星库，包含极限星等6.0Mv和7.5Mv的两个星库
	Table1：左上的列表为模拟星图时使用的星列表，包含恒星在星库中的序号，质心坐标和星等信息。

3. Object / FakeStar 目标/假星
	Add：在左下的列表中添加一个目标/假星，并在图中显示。位置随机，等效星等和速度按照"3."中设置的值，增益和曝光时间按照"5."中设置的值。
	Delete：删除所有目标/假星，并在图像中使用背景均值抹去目标/假星。
	Mv Vx Vy：设置待添加目标/假星的等效星等、X方向速度和Y方向速度。
	Table2：左下的列表为图像中添加目标/假星的列表，包含质心坐标、速度和等效星等信息。

4. Camera_Parameter 相机参数设置
	NumX, NumY：像素数 单位为 (pixel)
	CenX, CenY：主点坐标 单位为 (pixel)
	DX, DY：传感器像元尺寸 单位为 (mm)
	Focal：相机焦距 单位为 (mm)
	FOV：图像对角线对应的视场角 单位为 (°)

5. Image_Parameter 图像参数设置
	Mode：可以选择statics静态和Dynamic动态模式，动态模式假设相机匀速直线运动，将根据"5."中的曝光时间和速度生成动态星图。生成动态星图的时间比静态稍长一些。
	Clean按钮：清除UI界面中间的图像。
	Simulation按钮：按照左上侧的恒星列表中质心位置和星等信息，并以"5."中的增益生成恒星图像，恒星的成像点扩散函数遵循高斯函数。并按照"5."中的背景均值和方差生成符合正态分布的背景。Simulation过程只生成星图，与"3."中的目标/假星添加过程互相独立。
	Exposure：曝光时间，单位为 (frame帧)，原理为多张星图叠加
	Vx, Vy：动态模式时 相机运动的速度 单位为 (pixel/frame)
	Gain_star：增益，可以按比例调整所有恒星和目标的亮度，同时被"3."中恒星和"5."中目标/假星使用。
	Avg_back, Std_back：背景的均值和标准差

6. Star_Extraction 星点提取
	Star_Extraction按钮：按照灰度阈值对图像进行二值化分割，按照连通域阈值提取恒星/目标，展示在右上的表格中。
	Gray_thres：灰度阈值，单位为灰度，取值范围1-255
	Pixel_thres：连通域阈值，单位为像素
	Num_Star：星点数量，会按照检测/识别结果自动更新。与右上的列表保持一致

7. Star_Identification 星图识别
	Iden按钮：星图识别，将灰度求和排名前20的星进行识别，恒星结果展示在右上的表格中，并在图像相应的位置作蓝色标记。目标/假星结果展示在右下的表格中，并在图像相应的位置作红色标记。计算姿态展示在"7."中的Ra Dec Roll中。
	Ra Dec Roll：识别结果解算的姿态参数。也作为"8."中Projection反投影的姿态参数，此时可以根据反投影需要的参数手动输入。
	Table3：右下的表格，包含提取/识别的恒星质心坐标、灰度和、星编号信息
	
8. Other_Functions 其他功能
	Import Image按钮：导入图像，若需要识别指定星图，可以通过该功能按钮选择星图文件，可以是模拟星图或真实拍摄的星图。导入星图后"4."中的图像尺寸会自动更新，主点位置默认为图像中心，若有高精度标定结果，可以修改。若希望星图识别成功，需要准确设置"4."中的像元尺寸和相机焦距。
	Save Image按钮：保存图像，可以将UI界面中间展示的图像保存至指定路径，保存结果不包含图窗上的标记。
	
	Flip_UpDown按钮 ：上下翻转图像
	Flip_LiftRight按钮 ：左右翻转图像

	Projection按钮：反投影，根据"7."中的姿态参数和"4."中的相机参数，搜索星库中位于视场内的星，更新在右上的表格中，并在图像相应的位置作绿色标记
	Mask按钮：按照右上列表中的恒星质心坐标和预先设定的大小，对恒星进行掩膜处理。

9. Object/FakeStar Detection 目标/假星探测
	Detect按钮：目标检测，检测结果展示在右下的表格中，并在图中红色标记。
	Gray_thres：灰度阈值，单位为灰度，取值范围1-255
	Pixel_thres：连通域阈值，单位为像素
	Num：目标数量，会按照检测/识别结果自动更新。

	*Todo：目前仅通过阈值分割方法检测能力弱，后续需要考虑更有效的检测方法。

