# Household object detection by right hand's index fingertip
### Members <br/>
Group Wall-E 2<br/>
6330301321 Panithi Kamwangyang <br/>
6330305921 Pras Pitasawad <br/>
6330563421 Akira Sitdhikariyawat <br/>

## Instructions
The main file is Object Detection from Fingertip.ipynb. Run the file to run the application.<br/>
All other previous attempt of stereovision approach/others is located in folder prototype.<br/>

## Materials
### Presentation
Presentation Slide: <br/>
Full Slide: https://github.com/bbompk/onlaweng/blob/main/presentation.pdf<br/>
1 Page Slide: https://github.com/bbompk/onlaweng/blob/main/presentation%201page.pdf<br/>
Video: <br/>
Video Presentation & Demo: https://youtu.be/fyVjunwzBFo<br/>
### Project Description
more information on presentation slide and video

### Problem statement
เป็นการทำ basic household object detection from index fingertip หรือก็คือการระบุของที่ต้องการจะ identify ผ่านการชี้ด้วยนิ้วชี้ของมือขวา ซึ่ง input ก็คือภาพวิดิโอที่ถ่ายอยู่ที่ scene หนึ่ง (fixed camera position) ประกอบกับ movement ของมือเรา output ก็คือการที่เมื่อนิ้วเราชี้ไปตรง object ใด program เราต้องระบุว่าเราชี้ไปที่ ob่ject ไหน

ซึ่งเราหวังว่าการทำ project นี้จะสามารถนำไปใช้ใน application ต่างๆในชีวิตจริงๆได้ ยกตัวอย่างเช่นในกรณีของผู้พิการ การที่เราสามารถระบุสิ่งของได้เพียงการชี้นิ้ว ทำให้สามารถนำสิ่งของที่ถูกชี้ไปใช้งานต่อได้ เช่นให้หุ่นยนต์ช่วยหยิบของชิ้นนั้นให้ หรืออาจเป็นการนำไปใช้ในร้านค้าต่างๆ เพื่อชี้ระบุในการทำ automatic shopping system โดยที่คนไม่ต้องถือของเองได้ เป็นต้น

### Technical challenges
หนึ่งใน challenge หลักของพวกเราคือการที่วัตถุอยู่ในภาพ ทำให้เราเห็นได้เพียงเเค่พิกัด x,y เเต่การที่เราชี้นิ้วจริงๆมันจะมีความลึก (หรือ z) เข้ามาเกี่ยวข้องได้ ทำให้เราต้องสามารถรู้ให้ได้ว่า จริงๆเเล้วเเต่ละ object ในภาพมี x,y,z เป็นเท่าใด 

นอกจากนั้นยังมี challenge อื่นๆ เช่นการทำให้ responsive ทำให้ผู้ใช้งานไม่รู้สึกว่ากระตุก หรือว่า robustness เช่นลักษณะมือของคนชี้ที่ไม่เหมือนกัน หรือว่าจะเป็น object เดียวกันอาจมีสีหรือรูปร่างที่เเตกต่างกันออกไป

### Related works
ในการศึกษา การที่เราจะระบุ depth ของ object ผ่านการใช้มุมมองจากกล้องตัวเดียวนั้นเป็นไปไม่ได้ ต้องใช้อย่างต่ำ 2 กล้องเข้ามาช่วย ซึ่่งหนึ่งในวิธีการนั้นคือการทำ stereovision เพื่อหา x,y,z จากนั้นเมื่อได้ x,y,z จึงนำไปใช้งานต่อได้ในการคำนวณต่างๆ เพื่อระบุว่า เราชี้ไปที่ object ไหน
ส่วนการทำ object detection และ handtracking สามารถใช้ library ที่มีคนได้ทำการ implement มาเเล้ว ซึ่งกลุ่มพวกเราเลือกใช้เป็น MediaPipe ทั้ง 2 ส่วน หลังจากนั้นค่อยนำมา intregate รวมกันเป็นระบบ

### Method & Results
กลุ่มเราได้ลองทำ 2 วิธีคือ
(1) ทำแบบ Stereovision โดยที่ทำการ set กล้อง 2 ตัวให้ len อยู่ในเเนวขนานกันและสูงเท่ากัน ทำ camera calibration เพื่อนำไปคำนวณหา camera matrix โดยใช้ method จาก OpenCV ซึ่งพอได้ camera matrix นี้เเล้วจะสามารถนำไปคำนวณหาตำแหน่ง x,y,z ของ exoskeleton point ของมือคน และของ object ต่างๆในรุปได้ หลังจากนั้นจึงสามารถสร้าง projection line จากปลายนิ้วมือ เเล้วจึงทำการตรวจสอบว่า object ไหนอยุ่ใน project line ที่ชี้ เเล้วจึงนำมาแสดงเป็นผลลัพธ์ของ program

เราได้ลองวิธีนี้เเต่ไม่สามารถคำนวณหา camera matrix ที่ถูกต้องได้ นำมาสู่การคำนวณ x,y,z ที่ไม่ถูกต้อง ทำให้ progress ไปส่วนอื่นไม่ได้ ซึ่งเราคิดว่ามาจากการที่เรา calibrate camera ได้ไม่ดีพอ ไม่ครอบคลุมทุกจุด ซึ่งขั้นตอนนี้ค่อนข้างละเอียดอ่อนจึง เกิดความผิดพลาดได้ง่าย กับอีกเหตุผลหนึ่งคือข้อจำกัดของ hardware คือกล้อง 2 ตัวที่ใช้จำเป็นต้องเหมือนกัน รวมถึงต้องรู้ specification ของกล้องอีก เช่น focal length ซึ่งพอเราไม่รู้จุดนี้เเล้วใช้การประมาณเอาจาก standard value ทำให้เกิด ความผิดพลาดจากการคำนวณได้เช่นกัน
<br/>
(2) ทำแบบ 2 camera setup คือตั้งกล้อง 2 ตัวไว้เป็นมุมฉากกัน โดยที่ในเเต่ละกล้อง เมื่อได้พิกัด x,y ของนิ้วมือคนจาก MediaPipe จึงนำมาคำนวณหา projection line เพื่อตรวจสอบว่า object ใดอยู่ใน projection line นั้นเเล้วบ้าง จากนั้นจึงนำผลลัพธ์จากกล้องทั้ง 2 มา intersection เพื่อให้ได้ object ที่เราชี้จริงๆ จากนั้นจึงนำไปแสดงผล

เหตุผลที่ใช้วิธีนี้เเละใช้ 2 กล้องคือ การที่เราใช้กล้องเดียว เมื่อวัตถุวางในแนวเดียวกันเเต่ความลึกไม่เท่ากัน ภาพจากมุมมอง 2 มิติบางมุมจะทำให้เราเห็นเหมือนวัตถุ 2 ชิ้นนั้นอยู่ในตำแหน่งเดียวกัน เมื่อเราเพิ่มอีกกล้องในมุมมองที่เเตกต่างกันออกมา จะทำให้ครอบคลุมทุก viewpoint ที่เป็นไปได้ object ที่เราชี้จริงๆต้องถูกชี้โดยทั้ง 2 กล้อง ซึ่งก็คือการทำ intersection ของผลลัพธ์จากทั้ง 2 กล้องนั่นเอง

### Discussion and future work 
- ลองกลับไปใช้ method streovision (1) อีกครั้ง โดยปรับ setting การทำ camera calibration ให้ครอบคลุมทุกจุดที่กล้องถ่าย เพื่อให้ผลเเม่นยำขึ้น พอทำ stereovision จนได้ x,y,z ของวัตถุจริงเเล้ว ค่อยนำไปประมวลผลต่อด้วยวิธีการคิดแบบเดิม
- ลองทำเป็น extended view หรือภาพในมุมกว้างเพื่อให้เหมาะสมกับ application ในการใช้งานจริงๆ เพราะปกติเเล้วการติดกล้องส่วนใหญ่มักติดในมุมมองที่เห็นได้กว้างๆ ทำให้ object ต่างๆในรูปมีขนาดเล็กลงมาก ซึ่งพอทำแบบเป็นมุมนี้อาจมี challenge ต่างๆเพิ่มอีก เช่นถ้า object ชิ้นเล็กมากๆเเล้วจะมองออกหรือไม่ หรือการระบุตำแหน่งที่ชี้จะต้องเเม่นยำกว่าเดิม

## External Documentation
Hand Detection: <br/>
https://developers.google.com/mediapipe/solutions/vision/hand_landmarker/python <br/>
https://developers.google.com/mediapipe/solutions/vision/hand_landmarker/index#models <br/>
<br/>
Object Detection: <br/>
https://developers.google.com/mediapipe/solutions/vision/object_detector/python#configuration <br/>
https://developers.google.com/mediapipe/solutions/vision/object_detector#models <br/>



