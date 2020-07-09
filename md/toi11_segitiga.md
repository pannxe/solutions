โดยทั่วไป เราสามารถแก้โจทย์ที่ถามเกี่ยวกับการใส่วงเล็บให้ได้ค่าน้อยสุด/มากสุด/ตรงตามเงื่อนไขได้ด้วย Dynamic Programming แบบ Substring State นั่นคือ แต่ละช่องของตาราง DP จะแสดงถึงวิธีการแก้ปัญหาหากพิจารณาว่ามีข้อมูลนำเข้าเพียงแค่ช่วงที่ติดกันช่วงหนึ่งเท่านั้น

พิจารณาแต่ละชุดทดสอบ เพื่อความสะดวก
- จะเรียก bitstring ที่ให้มาว่า $s$ ซึ่งมีความยาวทั้งหมด $n$ ตัวอักษร ($n = |s|$)
- หากเขียนว่า $s_i$ จะหมายถึงตัวอักษร ณ ตำแหน่งที่ $i$
- หากเขียนว่า $s_{i..j}$ จะหมายถึง substring ทั้งหมดตั้งแต่ตำแหน่งที่ $i$ ถึง $j$

**นิยาม**: $dp_{l,r,c}$ มีค่าเป็นจริงก็ต่อเมื่อมีวิธีใส่วงเล็บให้สตริง $s_{l..r}$ ได้ผลลัพธ์ออกมาเป็น $c$ ($1 \leq l \leq r \leq n$, $0\leq c \leq 2$)

กรณี $l=r$ จะได้ $dp_{l,r,c}$ เป็นจริงก็ต่อเมื่อ $s_l = c$

กรณี $l < r$ หากเราต้องการทราบว่าสามารถใส่วงเล็บให้ได้ผลลัพธ์ $c=0$ หรือไม่ เราสามารถทดลองใส่วงเล็บแบ่งสตริงช่วง $[l,r]$ ออกเป็นสองช่วง $[l,x]$ และ $[x+1, r]$ ณ แต่ละตำแหน่ง $x$ ที่ $l \leq x < r$ แล้วตรวจสอบว่ามีวิธีทำให้ช่วงด้านซ้ายเป็นเลข $0$ และช่วงด้านขวาเป็นเลข $2$ ได้หรือไม่ ถ้ามีการแบ่งอย่างน้อยหนึ่งตำแหน่งที่ทำได้ แปลว่าเราสามารถทำให้สตริง $(s_{l..x}) \nabla (s_{(x+1)..r})$ ดำเนินการได้ $0 \nabla 2 = 0$ ตามที่เราต้องการ

สรุปเป็น recurrence formula ได้ว่า

$$dp_{l,r,0} = \bigvee\limits_{l \leq x < r}\left( dp_{l,x,0} \wedge dp_{x+1,r,2} \right)$$

สำหรับกรณี $c=1$ สามารถคิดได้คล้าย ๆ กัน แต่ว่ามีตัวเลือกในการสร้างผลลัพธ์ทางด้านซ้ายและทางด้านขวาหลายแบบ ได้แก่ $0 \nabla 1$, $1 \nabla 1$, $1 \nabla 2$, $2 \nabla 0$, และ $2 \nabla 2$ ซึ่งหากทำได้อย่างน้อยหนึ่งแบบก็ถือว่า $dp_{l,r,c}$ เป็นจริง

$$dp_{l,r,1} = \bigvee\limits_{l \leq x < r} \bigvee
\begin{cases}
  dp_{l,x,0} \wedge dp_{x+1,r,1} \\
  dp_{l,x,1} \wedge dp_{x+1,r,1} \\
  dp_{l,x,1} \wedge dp_{x+1,r,2} \\
  dp_{l,x,2} \wedge dp_{x+1,r,0} \\
  dp_{l,x,2} \wedge dp_{x+1,r,2} \\
\end{cases}
$$

ส่วนกรณี $c=2$ คือ

$$dp_{l,r,2} = \bigvee\limits_{l \leq x < r} \bigvee
\begin{cases}
  dp_{l,x,0} \wedge dp_{x+1,r,0} \\
  dp_{l,x,1} \wedge dp_{x+1,r,0} \\
  dp_{l,x,2} \wedge dp_{x+1,r,1}
\end{cases}
$$

หากสร้างตาราง $dp_{l,r,c}$ ด้วยสูตรดังกล่าว (สำหรับแบบ bottom-up จำเป็นต้องไล่ตั้งแต่ช่วงที่มีขนาดเล็กสุดไปยังช่วงที่มีขนาด $n$ ตัวอักษร) จะได้คำตอบเป็น "yes" เมื่อ $dp_{1,n,0}$ เป็นจริง มิเช่นนั้นตอบ "no"