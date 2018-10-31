# Computational-Physics
my homework
3.11 For the three values of FD shown in Figure 3.6, compute and plot the total energy of the system as a function of the time and discuss the results.

import math
import pylab as pl
class pendulum:
    def __init__(self, theta=0, time_step=0.04, g=9.8, l=1, w=0, init_t=0,
                damped=0, driven_force=0, wd=2/3):
        self.theta = [theta]
        self.dt = time_step
        self.g = g
        self.l = l
        self.w = [w]
        self.t = [0]
        self.wd = wd
        self.init_t = init_t
        self.damped = damped
        self.df = driven_force
        self.E = [0]
        print("Lenth =",self.l)
        print("Damped Coefficient =",self.damped)
        print("Driven force =",self.df)

    def calculate(self, duration = 60, m = 1):
        print("duration =", duration)
        self.E[-1] =0.5 * m * self.l**2 * self.w[-1]**2 + m * self.g * \
self.l * (1 - math.cos(self.theta[-1]))
        while self.t[-1] < duration:
            wtemp = self.w[-1] + ( - self.g / self.l * math.sin(self.theta[-1]) - \
self.damped * self.w[-1] + self.df * math.sin(self.wd * self.t[-1])) * self.dt
            self.w.append(wtemp) 
            thetatemp = self.theta[-1] + self.w[-1] * self.dt
            while thetatemp <  - math.pi:
                thetatemp += 2 * math.pi
            while thetatemp >  math.pi:
                thetatemp -= 2 * math.pi
            self.theta.append(thetatemp)
            self.t.append(self.t[-1] + self.dt)
            Etemp = 0.5 * m * self.l**2 * self.w[-1]**2 + m * \
self.g * self.l * (1 - math.cos(self.theta[-1]))
            self.E.append(Etemp)
    
    def show_result_E(self):
        pl.plot(self.t, self.E)
#        pl.ylim((0,10))
        pl.xlabel('time ($s$)')
        pl.ylabel('total energy')
        pl.show()
    def show_result_theta(self):
        pl.plot(self.t,self.theta)
        pl.xlabel('time($s$)')
        pl.ylabel('theta')
        pl.show()
    def show_result_w(self):
        pl.plot(self.t,self.w)
        pl.xlabel('time($s$)')
        pl.ylabel('w')
        pl.show()
        
        
a = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 0)
a.calculate()
a.show_result_E()
aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 0.5)
aa.calculate()
aa.show_result_E()
aaa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aaa.calculate()
aaa.show_result_E()   
#由上面三个图可初步推测出加入系统的外力driven force在一定范围会使系统进入混沌态，体系能量不能稳定下来
#下面尝试增加体系持续时间，观察总能量变化

a = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 0)
a.calculate(duration = 200)
a.show_result_E()
aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 0.5)
aa.calculate(duration = 200)
aa.show_result_E()
aaa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aaa.calculate(duration = 200)
aaa.show_result_E()

#上面程序显示将时间增加至 200 后的结果，
#可看出driven force = 1.2 的体系依然处于混沌状态，未进入混沌状态的体系总能量稳定变化
#故时间对混沌态没有影响

#下面分析driven force对体系的影响

a = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.1)
a.calculate(duration = 400)
a.show_result_E()
aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aa.calculate(duration = 400)
aa.show_result_E()
aaa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.3)
aaa.calculate(duration = 400)
aaa.show_result_E()

#由上图可以看出改变driven force的确会对体系总能量是否稳定产生较大影响，driven force在确定范围能将使系统进入混沌态



3.13 Write a program to calculate and compare the behavior of two, nearly identical pendulums. Use it to calculate the divergence of two nearby trajectories in the chaotic regime, as in Figure 3.7, and make a qualitative estimate of the corresponding Lyapunov exponent from the slope of a plot of log(Δθ) as a function of t.
#由上面的题目可知两个混沌态的初始状态为
#theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2
#或theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.1
#下面就theta、damped、l、driven—force等分析定性估计Lyapunov指数

#先小幅改变theta以对混沌态有个初步认识
a = pendulum(theta=0.2, damped=0.5, driven_force=1.2, l=9.8)
a.calculate(duration=100)
aa = pendulum(theta=0.3, damped=0.5, d
riven_force=1.2, l=9.8)
aa.calculate(duration=100)
pl.plot(a.t,a.theta,aa.t,aa.theta)
pl.show()

#可见微小的初始theta改变已使 t=20 后的theta几乎完全没有关联  
#下面展示d——theta与时间的图像关系

aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aa.calculate(duration = 200)
aaa = pendulum(theta = 0.199, damped = 0.5, l = 9.8, driven_force = 1.2)
aaa.calculate(duration = 200)
def lyapunov(self1, self2):
    dtheta = []
    ttheta = 0
    for i in range(len(self1.t)):
        ttheta = abs(self1.theta[i] - self2.theta[i])
        dtheta.append(ttheta)
    pl.plot(self1.t,dtheta)
    pl.show()
l = lyapunov(aa,aaa)

aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aa.calculate(duration = 200)
aaa = pendulum(theta = 0.21, damped = 0.5, l = 9.8, driven_force = 1.2)
aaa.calculate(duration = 200)
def lyapunov(self1, self2):
    dtheta = []
    ttheta = 0
    for i in range(len(self1.t)):
        ttheta = math.log(abs(self1.theta[i] - self2.theta[i]),10)
        dtheta.append(ttheta)
    pl.plot(self1.t,dtheta)
    pl.show()
l = lyapunov(aa,aaa)

#由上图可看出计算所得lyapunov指数有大于0的趋势，这表明系统出现了混沌
#下面尝试改变绳长 l 来讨论Lyapunov指数

#这种情况下会出现log（0）的错误，故加入try expect 捕获异常，改奇点为1，并画出图像
aa = pendulum(theta = 0.2, damped = 0.5, l = 9.8, driven_force = 1.2)
aa.calculate(duration = 200)
aaa = pendulum(theta = 0.2, damped = 0.5, l = 10, driven_force = 1.2)
aaa.calculate(duration = 200)
def lyapunov(self1, self2):
    dtheta = []
    ttheta = 0
    i = 0
    while i <len(self1.t):
        try:
            ttheta = math.log(abs(self1.theta[i] - self2.theta[i]),10)
            dtheta.append(ttheta)
        except:
            print("除去一个奇点")
            dtheta.append(1)
        finally: i += 1
    
    pl.plot(self1.t,dtheta)
    pl.show()
l = lyapunov(aa,aaa)

#由上面的例子可以看出混沌状态下运动的theta差别不断变大，由Lyapunov指数大于0，可确定其为混沌态，与实际相符
