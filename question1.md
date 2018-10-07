# Computational-Physics
my homework

1.Use the Euler method to calculate cannon shell trajectories ignoring both air drag and the effect of air density (actually, ignoring the former automatically rules out the latter). Compare your results with those in Figure 2.4, and with the exact solution.

import math
import pylab as pl
pi = math.pi
class cannon:
    def __init__(self, time_step = 0.01, init_x_label = 0, 
                init_y_label = 0, launch_angle = 60, g = 9.8,
                init_v =100):
        #定义所需要素
        self.g = g
        self.dt = time_step
        self.x_label = [init_x_label]
        self.y_label = [init_y_label]
        self.vx = init_v * math.cos(launch_angle * pi/180)
        self.vy = init_v * math.sin(launch_angle * pi/180)
        print("initial velocity ->", init_v)
        print("initial height ->", init_y_label)
        print("launch angle ->", launch_angle)
        print("time step ->", time_step)
    def calculate(self):
        while self.y_label[-1] >= 0 :
            Xtmp = self.x_label[-1] + self.vx * self.dt
            self.x_label.append(Xtmp)
            self.vy = self.vy - self.g * self.dt
            Ytmp = self.y_label[-1] + self.vy * self.dt
            self.y_label.append(Ytmp)
        #Euler法解微分方程
        r = -self.y_label[-2]/self.y_label[-1]
        self.x_label[-1] = (self.x_label[-2] + r * self.x_label[-1]) / (r + 1)
        self.y_label[-1] = 0
        #确定落点位置
a40 = cannon(launch_angle = 40)
a40.calculate()
a45 = cannon(launch_angle = 45)
a45.calculate()
a50 = cannon(launch_angle = 50)
a50.calculate()
a55 = cannon(launch_angle = 55)
a55.calculate()
a60 = cannon(launch_angle = 60)
a60.calculate()
pl.plot(a40.x_label,a40.y_label,a45.x_label,a45.y_label,a50.x_label,a50.y_label,
        a55.x_label,a55.y_label,a60.x_label,a60.y_label)
#将几条不同角度的线画在一张图上
pl.show()
