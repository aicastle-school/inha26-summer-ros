# [2] Limo Pro 기초

- 2026년 06월 23일 (화) 교육 자료
- Limo Pro의 기초 사용법 숙지
- [Limo Pro 메뉴얼](https://goofy-pleasure-a84.notion.site/Limo-Wiki-a6aa65b627cb40019a82d469dc5ae69d?pvs=4)

## 2.1. 원격 접속

<img src="images/image1.png" width="600">

### 2.1.1 vscode (ssh)

1. Remote Development 설치

   <img src="images/image2.png" width="600">

2. 원격 탐색기 - ssh 대상 - 추가(+) → ssh 접속 명령어 입력 → 패스워드 입력  

   <img src="images/image3.png" width="600">

3. 탐색기 - 폴더 열기

   <img src="images/image4.png" width="600">

4. 패키지 작업 진행(코드 작성 / 터미널 실행)  
   
    <img src="images/image5.png" width="600">

## 2.1.2. NoMachine

1. NoMachine 설치  
2. 자동 검색으로 나오면 바로 접속  
3. 안 나오는 경우 Add 클릭 \- Add Connection  

   <img src="images/image6.png" width="600">

4. Name(아무거나), Host(IP 주소) 작성  

   <img src="images/image7.png" width="600">  
   
5. 접속(Verify host identification 뜰 시 OK 클릭)  

   <img src="images/image8.png" width="600">

6. ubuntu 아이디 및 패스워드 입력 후 접속  

   <img src="images/image9.png" width="600">  
7. CompressedImage가 나오지 않을때 

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image, CompressedImage
from cv_bridge import CvBridge
import cv2

class ImageCompressorNode(Node):
    def __init__(self):
        super().__init__('image_compressor_node')
        
        # OpenCV 변환 브릿지
        self.bridge = CvBridge()
        
        # 압축 품질 설정 (1~100, 낮을수록 용량 작고 화질 나쁨)
        self.jpeg_quality = 30
        
        # 원본 이미지 구독 (Subscribe)
        self.subscription = self.create_subscription(
            Image,
            '/camera/color/image_raw',
            self.image_callback,
            10)
            
        # 압축 이미지 발행 (Publish)
        self.publisher = self.create_publisher(
            CompressedImage,
            '/camera/color/image_raw/compressed',
            10)
            
        self.get_logger().info('Image Compressor Node가 시작되었습니다.')
        self.get_logger().info(f'타겟 토픽: /camera/color/image_raw -> 압축 품질: {self.jpeg_quality}%')

    def image_callback(self, msg):
        try:
            # 1. ROS Image 메시지를 OpenCV 이미지(numpy 배열)로 변환
            cv_image = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
            
            # 2. OpenCV를 이용해 JPEG 포맷으로 압축
            encode_param = [int(cv2.IMWRITE_JPEG_QUALITY), self.jpeg_quality]
            result, encimg = cv2.imencode('.jpg', cv_image, encode_param)
            
            if not result:
                self.get_logger().error("이미지 압축에 실패했습니다.")
                return

            # 3. CompressedImage 메시지 생성 및 데이터 복사
            compressed_msg = CompressedImage()
            compressed_msg.header = msg.header  # 원본 이미지의 시간, 프레임 정보 유지
            compressed_msg.format = "jpeg"
            compressed_msg.data = encimg.tobytes() # 압축된 바이트 데이터 삽입
            
            # 4. 압축된 이미지 발행
            self.publisher.publish(compressed_msg)
            
        except Exception as e:
            self.get_logger().error(f'콜백 처리 중 오류 발생: {str(e)}')


def main(args=None):
    rclpy.init(args=args)
    node = ImageCompressorNode()
    
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        node.get_logger().info('사용자에 의해 노드가 종료됩니다.')
    finally:
        node.destroy_node()
        rclpy.shutdown()

if __name__ == '__main__':
    main()
```
