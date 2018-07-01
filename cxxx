// Action.cpp: implementation of the Action class.
// 动作函数的实现
//////////////////////////////////////////////////////////////////////

#include "Action.h"
#include "Game.h"
#include "function.h"
#include "math.h"

//////////////////////////////////////////////////////////////////////
// Construction/Destinationruction
//////////////////////////////////////////////////////////////////////




//在game里面 修改robot的左右轮速vl,vr  这里是间接的/
void Velocity(Game *g, int robot,double vl,double vr)
{
	if(g->robot[robot].deel==true)////避免重复处理
		return;
	vl=Cut(vl,-125,125);
	vr=Cut(vr,-125,125);

	if(vl==0 && vr!=0)
		vl=0.00001;
	if(vr==0 && vl!=0)
		vr=0.00001;

	g->robot[robot].velocityLeft = vl;
	g->robot[robot].velocityRight= vr;
	g->robot[robot].deel=true;

}
//在game里面 修改robot的左右轮速vl,vr  这里是间接的/
void Velocity2(Game *g, int robot,double vl,double vr)
{
	vl=Cut(vl,-125,125);
	vr=Cut(vr,-125,125);

	if(vl==0 && vr!=0)
		vl=0.00001;
	if(vr==0 && vl!=0)
		vr=0.00001;

	g->robot[robot].velocityLeft = vl;
	g->robot[robot].velocityRight= vr;
	g->robot[robot].deel=true;

}

//转到angle的方向
void Angle( Game *g, int robot,double angle)
{
	double speed = 0;		
	double accuracy=1;//accuracy:精度^^
	double turnangle=0,nextangle=0;
	double FF=125;		//最大减速度

	turnangle = angle -g->robot[robot].rotation;//所需要转的角度^^
	NormalAngle(turnangle);

	if(turnangle < 1 && turnangle >-1)
	{
		Velocity(g,robot,0,0);
		return ;
	}
	else if(turnangle < 2 && turnangle >-2)
		FF=10;
	else if( turnangle >-3 && turnangle < 3)
		FF=15;
	else if( turnangle >-5 && turnangle < 5)
		FF=30;

	double v=g->robot[robot].rotation - g->robot[robot].oldpos.z ;//相邻调用间角度的变化值^^
	NormalAngle(v);
	double v1=v;
	double f=0;	

	bool turnleft=true;			
	//--判断是否向左转，画个图就很好理解了^^
	if(turnangle > 90)
	{
		turnleft=false;
		turnangle-=180;
	}
	else if(turnangle >0)
	{
		turnleft=true;	
	}
	else if(turnangle > -90)
	{
		turnleft=false;	
	}
	else 
	{
		turnleft=true;
		turnangle+=180;		
	}
	//---------------------------------end^^
	if(turnleft)//向左转
	{//
		f=-FF;

		v1=AngleOne(v1,speed+f,speed-f);		//v1+=a *( -b *f-v1);//为了节约时间赋予最大轮速^^
		nextangle+=v1;							//个人感觉nextangle=g->robot[robot].rotation^^

		do{
			v1 =AngleOne(v1,speed-f,speed+f);//+= a *( b *f-v1);		// v1   
			nextangle+=v1;
		}while( v1 > 0  );

		nextangle-=v1;

		if(nextangle < turnangle)
		{
			Velocity(g,robot,speed+f,speed-f);
		}
		else 
		{

			v1 = AngleOne(v,speed-f,speed+f);  
			if( v1 < 0 )
			{
				do{
					f++;
					v1 = AngleOne(v,speed-f,speed+f);  
				}while( v1 < turnangle && f <FF);
			}
			Velocity(g,robot,speed-f,speed+f);
		}		
	}
	else
	{//
		f=FF;
		v1=AngleOne(v1,speed+f,speed-f);		
		nextangle+=v1;

		do{
			v1 =AngleOne(v1,speed-f,speed+f);
			nextangle+=v1;
		}while( v1 < 0 );

		nextangle-=v1;

		if(nextangle > turnangle)
		{
			Velocity(g,robot,speed+f,speed-f);
		}
		else 
		{
			v1 = AngleOne(v,speed-f,speed+f);  
			if( v1 > 0 )
			{
				do{
					f--;
					v1 = AngleOne(v,speed-f,speed+f);  
				}while( v1 > turnangle && f >-FF);
			}
			Velocity(g,robot,speed-f,speed+f);
		}		
	}

}


//让robot朝angle的方向跑，并且speed控制它的最大轮速
void Run_Angle( Game *g, int robot,double angle,double speed)
{

	double accuracy=1;
	double turnangle=0,nextangle=0;
	turnangle = angle -g->robot[robot].rotation;
	NormalAngle(turnangle);
	double v=g->robot[robot].rotation - g->robot[robot].oldpos.z ;
	NormalAngle(v);
	double v1=v;
	double FF=125;		
	double f=0;	//相当于减速时,右轮速度，

	bool turnleft=true;	//判断小车是否是该向左转		

	bool face ;
	if(  turnangle < 90 && turnangle > -90 )	//检查是否正面跑位
	{
		face = true;
		speed = speed;
	}
	else
	{
		face = false;
		speed = -speed;
	}



	if(turnangle > 90)
	{
		turnleft=false;
		turnangle-=180;
	}
	else if(turnangle >0)
	{
		turnleft=true;	
	}
	else if(turnangle > -90)
	{
		turnleft=false;	
	}
	else 
	{
		turnleft=true;
		turnangle+=180;		
	}



	if(turnleft)
	{//向左
		f=-FF;

		v1=AngleOne(v1,speed+f,speed-f);	
		nextangle+=v1;

		do{
			v1 =AngleOne(v1,speed-f,speed+f);
			nextangle+=v1;
		}while( v1 > 0  );//直到小于0为止

		nextangle-=v1;

		if(nextangle < turnangle)//不减速，go on
		{
			Velocity(g,robot,speed+f,speed-f);
		}
		else //过了，因该减速了
		{

			v1 = AngleOne(v,speed-f,speed+f);  
			if( v1 < 0 )
			{
				do{
					f++;
					v1 = AngleOne(v,speed-f,speed+f);  
				}while( v1 < turnangle && f <125);
			}
			Velocity(g,robot,speed-f,speed+f);
		}		
	}
	else//向右的就反过来
	{
		f=FF;
		v1=AngleOne(v1,speed+f,speed-f);		//v1+=a *( -b *f-v1);
		nextangle+=v1;

		do{
			v1 =AngleOne(v1,speed-f,speed+f);//+= a *( b *f-v1);		// v1   
			nextangle+=v1;
		}while( v1 < 0 );

		nextangle-=v1;

		if(nextangle > turnangle)
		{
			Velocity(g,robot,speed+f,speed-f);
		}
		else 
		{
			v1 = AngleOne(v,speed-f,speed+f);  
			if( v1 > 0 )
			{
				do{
					f--;
					v1 = AngleOne(v,speed-f,speed+f); 
				}while( v1 > turnangle && f >-125);
			}
			Velocity(g,robot,speed-f,speed+f);
		}		
	}
}


// 跑到并且停止
void Position(Game *g,int robot, Vector3D pos , double bestangle,double limit) //bestangle:调整后最好的角度值^^
{
	double anglespeedmax=0;		//控制转交速度的变量
	double vmax=125;
	double Limitedangle=2;		//这个是角度，2度   

	if( limit < 0.5 )//  默认减速范围
		limit =0.5;
	double Limiteddis=limit;	//减速范围有一个下限0.5，保证不会来回跑动

	double  distance;			//robot和目标点的距离
	double turnangle,posangle,vangle;	//转动角度 ，目标点相对robot的角度，速度的绝对角度
	double dx,dy;				//pos  和robot的坐标差
	double v,v1;				//临时保存速度的大小!!!
	double f=vmax;				//加速度变量
	double s=0;					//预测的减速位移(路程)
	int n=0;					//跑位的步数
	bool face=true;			//判断小车是否是正面前进

	v= g->robot[robot].speed.v;

	dx = pos.x - g->robot[robot].pos.x ;		//pos  和robot的坐标差
	dy = pos.y - g->robot[robot].pos.y ;

	distance = Distance(g->robot[robot].pos , pos);//dxp直线距离
	posangle = Atan(dy,dx);//dxp相对角度

	turnangle = g->robot[robot].rotation - posangle;		//转动角度 
	NormalAngle(turnangle);//dxp规范角度 使之在 [-180 ， 180] 区间 

	if(turnangle > 90)//dxp判断转动角度是否在90和-90直接，是则小车正面前进，否则转动角度改变180度
	{//判断小车是否是正面前进
		face=false;
		turnangle-=180;
	}
	else if(turnangle < -90)
	{
		face=false;
		turnangle+=180;
	}
	else 
	{
		face=true;
	}

	vangle = g->robot[robot].speed.z - g->robot[robot].rotation;	//dxp速度的绝对角度 ？
	NormalAngle(vangle);					
	if( vangle <-90 || vangle > 90 )//dxp控制在正负90之间		
		v=-v;

	if(face && v < -SPEED_ZERO || !face && v > SPEED_ZERO) //初速度方向不对，先减速
	{//dxp方向相同速度是反的   或   者方向相反速度是正的
		Velocity(g,robot,0,0);//减速到0
		return ;
	}

	v1=v;	

	if(distance > Limiteddis )//开始冲啊    Limiteddis==0.5
	{
		if(turnangle > Limitedangle || turnangle < -Limitedangle)	//角度不好，先调整角度
		{//Limitedangle==2

			if(turnangle > 20  || turnangle < -20)//dxp转动角度越大，控制转交速度的变量越小？
				anglespeedmax = 0;
			else if(turnangle > 10  || turnangle < -10)
				anglespeedmax = 125;
			else if(turnangle > 5  || turnangle < -5)
				anglespeedmax = 180;
			else 
				anglespeedmax = 200;

			Run_Angle(g,robot,posangle,anglespeedmax);//dxp 让robot朝angle的方向跑，并且speed控制它的最大轮速
		}
		else//角度OK，以最大速度冲
		{
			if(face)
				Velocity(g,robot,f,f);
			else
				Velocity(g,robot,-f,-f);
		}
	}
	else//比较近了
	{
		if(distance > 1)//但不是很近，角度不好，调整角度
		{		
			if(turnangle > Limitedangle || turnangle < -Limitedangle)
			{	
				Angle(g,robot,posangle);
				return ;
			}
		}

		if(distance < 0.4)//太近了，跑位后停止之后的调整角度
		{	
			if( v<0.1 && v>-0.1)
			{
				if(bestangle == 0)//不需要调整角度
					Velocity(g,robot,0,0);
				else//需要调整
					Angle(g,robot,bestangle);
				return	;
			}
		}


		vmax=125;
		if(face)//开始刹车了，正方向的考虑
		{
			f=-vmax;		
			v1=VelocityOne(v1,-f,-f);	//
			s=v1;//s为最大刹车，滑行的距离
			do{
				if(v1 > SPEED_ZERO)
					v1=VelocityOne(v1,0,0);
				else 
					v1=VelocityOne(v1,f,f);
				s+=v1;
			}while( v1 > 0 );

			s-=v1;

			if(s < distance)//可以的话
			{
				Velocity(g,robot,-f,-f);
			}
			else//不能以那个刹车
			{
				if(v > SPEED_ZERO)//速度稍大
					Velocity(g,robot,0,0);
				else
				{
					v1=VelocityOne(v,f,f);	
					if( v1 < 0 )
					{
						do{
							f++;		
							v1 = VelocityOne(v,f,f);
						}while( v1 < distance && f < vmax);	
					}
					Velocity(g,robot,f,f);
				}
			}
		}			
		else//反向的，跑过了？
		{
			f=vmax;		
			v1=VelocityOne(v1,-f,-f);
			s=v1;
			do{
				if(v1 < -SPEED_ZERO)	
					v1=VelocityOne(v1,0,0);
				else 
					v1=VelocityOne(v1,f,f);
				s+=v1;
			}while( v1 < -0.1 );

			s-=v1;

			if(s > -distance)
			{
				Velocity(g,robot,-f,-f);
			}
			else
			{
				if(v < -SPEED_ZERO)
					Velocity(g,robot,0,0);
				else
				{
					v1=VelocityOne(v,f,f);		
					if( v1 > 0 )
					{
						do{
							f--;		
							v1 = VelocityOne(v,f,f);
						}while( v1 > -distance && f > -vmax);	
					}
					Velocity(g,robot,f,f);
				}
			}
		}	
	}
}



void PositionAndStopX(Game *g,int  robot,Vector3D pos ,double Xangle,double limit) 
{



	double anglespeedmax=0;		//控制转交速度的变量
	double vmax=125;
	double Limitedangle=2;

	if( limit < 2 )
		limit =2;
	double Limiteddis=limit;

	double  distance;
	double turnangle,posangle,vangle;
	double dx,dy;
	double a=SPEED_A;
	double b=SPEED_B;
	double v,v1;
	double f=vmax;
	double s=0;
	int n=0;
	bool face=true;			//判断小车是否是正面前进

	v= sqrt(g->robot[robot].speed.x * g->robot[robot].speed.x + g->robot[robot].speed.y*g->robot[robot].speed.y);

	dx = pos.x - g->robot[robot].pos.x ;
	dy = pos.y - g->robot[robot].pos.y ;

	distance = Distance(g->robot[robot].pos , pos);
	posangle = Atan(dy,dx);

	turnangle = g->robot[robot].rotation - posangle;		//think more!!
	NormalAngle(turnangle);

	if(turnangle > 90)
	{
		face=false;
		turnangle-=180;
	}
	else if(turnangle < -90)
	{
		face=false;
		turnangle+=180;
	}
	else 
	{
		face=true;
	}

	vangle = g->robot[robot].speed.z - g->robot[robot].rotation;
	NormalAngle(vangle);
	if( vangle <-90 || vangle > 90 )
		v=-v;
	v1=v;

	if(distance > Limiteddis )//无所忌惮的跑吧
	{//it is too early to count the steps
		if(turnangle > Limitedangle || turnangle < -Limitedangle)
		{//adjust angle
			/////////////////测试这一段
			if(turnangle > 20  || turnangle < -20)
				anglespeedmax = 0;
			else if(turnangle > 10  || turnangle < -10)
				anglespeedmax = 125;
			else if(turnangle > 5  || turnangle < -5)
				anglespeedmax = 180;
			else 
				anglespeedmax = 200;
			///////////////测试这一段
			Run_Angle(g,robot,posangle,anglespeedmax);

		}
		else//角度很小，几乎是直线
		{
			if(face)
				Velocity(g,robot,f,f);
			else
				Velocity(g,robot,-f,-f);
		}//it is time to rush
	}
	else//呵呵，太近了，控制一下
	{
		if(distance > 1)
		{		//调整角度	return!!!!!!
			if(turnangle > Limitedangle || turnangle < -Limitedangle)
			{	
				Angle(g,robot,posangle);
				return ;
			}

		}

		if(distance < 1)
		{	//停止并转向		return!!!!!!
			if( v<0.5 && v>-0.5)
			{
				Velocity(g,robot,-Xangle,Xangle);
				return	;
			}
		}

		if(true)
		{
			vmax=125;
			if(face)
			{
				f=-vmax;		//减速度  为  0000000
				v1=VelocityOne(v1,-f,-f);		//加速一步
				s=v1;
				do{//whether to reduce
					if(v1 > SPEED_ZERO)	//as i said,this is limited
						v1=VelocityOne(v1,0,0);//dxp 使  v1==0.338v1
					else 
						v1=VelocityOne(v1,f,f); //dxp 使 v1==0.94v1 - 0.11
					s+=v1;
				}while( v1 > 0 );

				s-=v1;

				if(s < distance)
				{//不满足减速条件加速
					Velocity(g,robot,-f,-f);
				}
				else
				{
					if(v > SPEED_ZERO)
						Velocity(g,robot,0,0);
					else
					{
						v1=VelocityOne(v,f,f);		//减速一步
						if( v1 < 0 )
						{
							do{//该降低功率了
								f++;		//f=-vmax;
								v1 = VelocityOne(v,f,f);
							}while( v1 < distance && f < vmax);	
						}
						Velocity(g,robot,f,f);
					}
				}
			}			
			else
			{
				f=vmax;		//减速度!!!!!
				v1=VelocityOne(v1,-f,-f);
				s=v1;
				do{//whether to reduce
					if(v1 < -SPEED_ZERO)	//as i said,this is limited
						v1=VelocityOne(v1,0,0);
					else 
						v1=VelocityOne(v1,f,f);
					s+=v1;
				}while( v1 < -0.1 );

				s-=v1;

				if(s > -distance)
				{//不满足减速条件加速
					Velocity(g,robot,-f,-f);
				}
				else
				{
					if(v < -SPEED_ZERO)
						Velocity(g,robot,0,0);
					else
					{
						v1=VelocityOne(v,f,f);		//减速一步
						if( v1 > 0 )
						{
							do{//该降低功率了
								f--;		//f=-vmax;
								v1 = VelocityOne(v,f,f);
							}while( v1 > -distance && f > -vmax);	
						}
						Velocity(g,robot,f,f);
					}
				}
			}	
		}
	}
}
// 守门员专用的 跑位函数
void GoaliePosition(Game *g,int  robot,Vector3D pos ,double bestangle,double limit) 
{
	double anglespeedmax=0;	
	double vmax=125;		
	double Limitedangle=2;	

	if( limit < 0.4 )
		limit =0.4;
	double Limiteddis=limit;

	double  distance;		
	double turnangle,posangle,vangle;	
	double dx,dy;				
	double v,v1;			
	double f=vmax;			
	double s=0;				
	int n=0;				
	bool face=true;			

	v= sqrt(g->robot[robot].speed.x * g->robot[robot].speed.x + g->robot[robot].speed.y*g->robot[robot].speed.y);
	dx = pos.x - g->robot[robot].pos.x ;		
	dy = pos.y - g->robot[robot].pos.y ;

	distance = Distance(g->robot[robot].pos , pos);
	posangle = Atan(dy,dx);

	turnangle = g->robot[robot].rotation - posangle;	
	NormalAngle(turnangle);

	if(turnangle > 90)
	{
		face=false;
		turnangle-=180;
	}
	else if(turnangle < -90)
	{
		face=false;
		turnangle+=180;
	}
	else 
	{
		face=true;
	}

	vangle = g->robot[robot].speed.z - g->robot[robot].rotation;		
	NormalAngle(vangle);				
	if( vangle <-90 || vangle > 90 )	
		v=-v;

	if(face && v < -SPEED_ZERO || !face && v > SPEED_ZERO) //初速度方向不对，先减速
	{
		Velocity(g,robot,0,0);
		return ;
	}

	v1=v;

	if(distance > Limiteddis )//太远了，可以随便跑
	{
		if(turnangle > Limitedangle || turnangle < -Limitedangle)//角度不好 
		{
			if(turnangle > 50  || turnangle < -50)
				anglespeedmax = 0;
			else if(turnangle > 30  || turnangle < -30)
				anglespeedmax = 80;
			else if(turnangle > 10  || turnangle < -10)
				anglespeedmax = 125;
			else if(turnangle > 5  || turnangle < -5)
				anglespeedmax = 180;
			else 
				anglespeedmax = 200;


			Run_Angle(g,robot,posangle,anglespeedmax);
		}
		else//角度OK，冲
		{
			if(face)
				Velocity(g,robot,f,f);
			else
				Velocity(g,robot,-f,-f);
		}
	}
	else//已经近了，
	{
		if(distance > 1)//不过不是很近，并且角度不合适，调整角度
		{	
			if(turnangle > Limitedangle || turnangle < -Limitedangle)
			{	
				Angle(g,robot,posangle);
				return ;
			}
		}

		if(distance < 0.4)//很近了 速度又很小 转向
		{	

			if( v<0.1 && v>-0.1)
			{	
				Angle(g,robot,bestangle);
				return	;
			}
		}

		vmax=125;
		if(face)//正朝向的  减速啊
		{
			f=-vmax;		
			v1=VelocityOne(v1,-f,-f);
			s=v1;
			do{
				if(v1 > SPEED_ZERO)	
					v1=VelocityOne(v1,0,0);
				else 
					v1=VelocityOne(v1,f,f);
				s+=v1;
			}while( v1 > 0 );

			s-=v1;

			if(s < distance)
			{
				Velocity(g,robot,-f,-f);
			}
			else
			{
				if(v > SPEED_ZERO)
					Velocity(g,robot,0,0);
				else
				{
					v1=VelocityOne(v,f,f);		
					if( v1 < 0 )
					{
						do{
							f++;		
							v1 = VelocityOne(v,f,f);
						}while( v1 < distance && f < vmax);	
					}
					Velocity(g,robot,f,f);
				}
			}
		}			
		else//反过来
		{
			f=vmax;		
			v1=VelocityOne(v1,-f,-f);
			s=v1;
			do{
				if(v1 < -SPEED_ZERO)	
					v1=VelocityOne(v1,0,0);
				else 
					v1=VelocityOne(v1,f,f);
				s+=v1;
			}while( v1 < -0.1 );

			s-=v1;

			if(s > -distance)
			{
				Velocity(g,robot,-f,-f);
			}
			else
			{
				if(v < -SPEED_ZERO)
					Velocity(g,robot,0,0);
				else
				{
					v1=VelocityOne(v,f,f);		
					if( v1 > 0 )
					{
						do{
							f--;		
							v1 = VelocityOne(v,f,f);
						}while( v1 > -distance && f > -vmax);	
					}
					Velocity(g,robot,f,f);
				}
			}
		}
	}
}


//快速冲向某一点,非常有用
void Through(Game *g,int robot,Vector3D pos ,double MAX)
{
	double anglespeedmax=0;	
	double  distance;
	double turnangle,posangle,vangle;
	double dx,dy;
	double v,v1;
	double s=0;
	int n=0;
	bool face=true;			

	v= g->robot[robot].speed.v;

	dx = pos.x - g->robot[robot].pos.x ;
	dy = pos.y - g->robot[robot].pos.y ;

	distance = Distance(g->robot[robot].pos , pos);
	posangle = Atan(dy,dx);

	turnangle = posangle - g->robot[robot].rotation;		
	NormalAngle(turnangle);

	if(turnangle > 90)
	{
		face=false;
		turnangle-=180;
	}
	else if(turnangle < -90)
	{
		face=false;
		turnangle+=180;
	}
	else 
	{
		face=true;
	}

	vangle = g->robot[robot].speed.z - posangle;
	NormalAngle(vangle);
	if( vangle <-90 || vangle > 90 )
		v=-v;
	v1=v;

	if(fabs(turnangle) > 2 )
	{
		if(fabs(turnangle) > 20)
			anglespeedmax = 0;
		else if(fabs(turnangle) > 10)
			anglespeedmax = 125;
		else if(fabs(turnangle) > 5)
			anglespeedmax = 180;
		else 
			anglespeedmax = 200;
		Run_Angle(g,robot,posangle,anglespeedmax);
	}
	else//角度太小
	{
		if(face)//向前
			Velocity(g,robot,MAX,MAX);
		else
			Velocity(g,robot,-MAX,-MAX);
	}

}
void Penalty( Game *g,int robot)
{
	dianqiu3(g);
	//dianqiu5(g);
	//return ;
	//dianqiu2(g);
	return;
	Kick(g,robot,g->robot[robot].BestBall,g->BestGate,4.5);
	return;
	static int  i=0;	
	if(g->robot[robot].ToBall.dis>5 && i==0)
		Through(g,robot,g->ball.pos,125);
	else
	{
		Velocity(g,robot,-125,125);
		i=1;
	}
	//		Position(g,robot,g->ball.pos,0,3
}


void PenaltyGoalie( Game *g,int robot)//点球中的守门员
{

	int id = -1;
	for (int i = 0; i < 5; i++)//dxp 找到离球最近的敌方机器人，就是敌方点球的队员
	{
		if (id == -1 || Distance(g->ball.pos, g->opp[id].pos) > Distance(g->ball.pos, g->opp[i].pos))
		{
			id = i;
		}
	}
	if (Distance(g->ball.pos, g->robot[0].pos) < Distance(g->ball.pos, g->opp[id].pos))//dxp如果自己守门员离球比 点球的敌方近
	{
		double angle = atan((g->ball.pos.y - g->robot[0].pos.y) / (g->ball.pos.x - g->robot[0].pos.x));
		Run_Angle(g, 0, angle, 125); //dxp跑向球
		return;
	}

	Vector3D go;
	bool can_in=false;
	double y=meet_y(g->ball.pos,g->ball.speed.z,FLEFT);//和球门线的交点
	if(fabs(g->ball.speed.z)>90 && y<GTOP+0.3 && y>GBOT-0.3 && g->ball.speed.v>0.2)//如果可以打进
		can_in=true;

	double x0;//守门员的x坐标,不可以轻易调整
	x0=g->robot[robot].pos.x;

	double gx=x0,gy;
	Vector3D ball=g->ball.pos;

	if(ball.x< x0+1.5 )//底线
	{
		if(ball.y<GTOP && ball.y>GBOT || ball.x< x0-1.5-0.2)//中间
		{
			Goalie_out(g,robot);
			return;
		}
		else//靠 上 或者 下 一点
		{
			gy=ball.y;
		}
	}
	else//中路
	{
		//如果只有守门员离球最近
		if(can_in)//如果可以打进
		{
			y=meet_y(g->ball.pos,g->ball.speed.z,x0-1.5);

			if(y>41.7+1.5)
				gy=y+0.5;
			else if(y<41.7-1.5)
				gy=y-0.5;
			else
				gy=y;
		}
		else//预防
		{
			FromTo gateto1=ball-Vect(FLEFT,GTOP,0);
			FromTo gateto2=ball-Vect(FLEFT,GBOT,0);
			double k=0.5;

			double angle=gateto1.z*(1-k)+gateto2.z*k;
			gy=ball.y+tan(angle*PI/180)*(x0 - ball.x);
		}
	}


	gy=Cut(gy,GBOT-3,GTOP+3);

	go.y = gy ;
	go.x = gx ;

	GoaliePosition(g,robot,go,90,0.2);

}

void PenaltyGoalie2( Game *g,int robot)//点球中的守门员
{
	if(Distance(g->robot[robot].pos,g->ball.pos)>RR+BR+1.0*BR/2)   //守门员和球的距离大于球半径+机器人半径
		Through(g,robot,g->ball.pos,125);		//向球冲
	else
	{
		Vector3D pos;
		pos.x=g->ball.pos.x-g->ball.oldpos.x;
		pos.y=g->ball.pos.y-g->ball.oldpos.y;
		if(pos.x>0&&pos.y>0)
			Velocity(g,robot,125,-125);
		else
			Velocity(g,robot,-125,125);
	}
}

void PenaltyGoalie3( Game *g,int robot)//点球中的守门员
{

}



void GoalieThroughBall(Game *g,int robot)//判断守门员是不是主动要去撞球
{
		if(Distance(g->robot[robot].pos,g->ball.pos)<=(2*RR)&&g->ball.pos.y<=(STOP-3*BR)&&g->ball.pos.y>=(SBOT+3*BR))
	{
//在球与守门员足够接近时,并且球的范围在小禁区区域时
		if(g->robot[robot].pos.x<g->ball.pos.x)//并且球在守门员前方时
		{
		Vector3D ToPos;
		ToPos.x=g->ball.pos.x+40;
		ToPos.y=g->ball.pos.y;
		ToPos.z=0.0;
		Kick(g , robot , ToPos );
		}
	}
}

//守门员一定要灵活，！！！！·····
void Goalie ( Game *g, int robot )
{
	GoalieThroughBall(g,robot);
	Vector3D go;
	bool can_in=false;
	double y=meet_y(g->ball.pos,g->ball.speed.z,FLEFT);//和球门线的交点
	if(fabs(g->ball.speed.z)>90 && y<GTOP+0.3 && y>GBOT-0.3)//如果可以打进
		can_in=true;

	double x0;//守门员的x坐标,不可以轻易调整
	x0=g->robot[robot].pos.x;

	if(g->ball.GateTo.dis + g->HisSmallDis>30)//非常安全区域,可以调整
		x0=FLEFT+2.0;
	else if(g->ball.GateTo.dis + g->HisSmallDis>20 && can_in==false)//
		x0=Cut(x0,FLEFT+1.8,FLEFT+2.2);

	double gx=x0,gy;
	Vector3D ball=g->ball.pos;
	int steps=toint((g->HisSmallDis)/2.5);

	PredictBall(g,steps);
	if(steps>20)
		steps=20;
	Vector3D preball=g->ball.prepos[steps];


	if(ball.x< x0+1.5 )//底线
	{
		if(ball.y<GTOP && ball.y>GBOT || ball.x< x0-1.5-0.2)//中间
		{
			Goalie_out(g,robot);
			return;
		}
		else//靠 上 或者 下 一点
		{
			gy=ball.y;
		}
	}
	else//中路
	{
		//如果只有守门员离球最近
		if(g->robot[robot].ToBall.dis<3.5 && ball.y<g->robot[robot].pos.y+1.5 && ball.y>g->robot[robot].pos.y-1.5 && g->HisSmallDis>20)
		{
			if(ball.y>41.7)
				Velocity(g,robot,-125,125);
			else
				Velocity(g,robot,125,-125);
		}
		if(can_in)//如果可以打进
		{
			y=meet_y(g->ball.pos,g->ball.speed.z,x0+1.5);

			if(y>41.7+1.5)
				gy=y-0.5;
			else if(y<41.7-1.5)
				gy=y+0.5;
			else
				gy=y;
		}
		else//预防
		{
			FromTo gateto1=preball-Vect(FLEFT,GTOP,0);
			FromTo gateto2=preball-Vect(FLEFT,GBOT,0);
			double k=gateto1.dis/gateto2.dis;
			k=k/(1+k);
			k=(k+0.5)/2;


			double angle=gateto1.z*(1-k)+gateto2.z*k;
			gy=preball.y+tan(angle*PI/180)*(x0 - preball.x);
		}
	}


	gy=Cut(gy,GBOT-3,GTOP+3);

	go.y = gy ;
	go.x = gx ;

	GoaliePosition(g,robot,go,90,0.2);
}



//门将的出击
void Goalie_out(Game *g,int robot)
{
	Vector3D ball = g->robot[robot].BestBall;		//这个球的位置不保险!!!
	Vector3D ToPos={50,ball.y,0};

	Speed ToBall;		//人和球的相对位置
	ToBall = ball - g->robot[robot].pos ;

	Speed BallToGate ;		//球和球门的相对位置
	BallToGate = ToPos - ball ;

	double RunAngle ;
	RunAngle = ToBall.z - BallToGate.z;
	NormalAngle(RunAngle);


	if(g->robot[robot].NextToBall.dis<3.2 && g->HisSmallDis> 4)
	{
		if(RunAngle<0)
			Velocity(g,robot,-125,125);
		else
			Velocity(g,robot,125,-125);
		return;
	}

	double xishu=0.5;
	xishu=ToBall.dis*(0.3-0.8)/20+0.8;

	RunAngle = ToBall.z + RunAngle * xishu;	// 可以调整  2 
	NormalAngle(RunAngle);

	Run_Angle(g,robot,RunAngle,125);

}


//预测经过 steps 个周期之后球的位置//
void PredictBall(Game *g,int steps)
{

	Vector3D predictball;
	Vector3D ballspeed;
	int i=0;

	predictball.x = g->ball.pos.x;			//赋初值
	predictball.y = g->ball.pos.y;

	ballspeed.x = g->ball.speed.x ;
	ballspeed.y = g->ball.speed.y ;
	ballspeed.z = g->ball.speed.z ;

	for(i=0;i<steps;i++)
	{
		predictball.x += ballspeed.x ;
		predictball.y += ballspeed.y ;

		if(predictball.x > FRIGHT && fabs(predictball.y-41.7)>9)//边界处理之反弹^^
		{
			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			ballspeed.x *=-SPEED_NORMAL;	//x属于反弹^^
			ballspeed.y *= SPEED_TANGENT;	//y属于正弹^^

			predictball.x += ballspeed.x ;	
			predictball.y += ballspeed.y ;

		}
		else if( fabs(predictball.y-41.7)<=9 )//特殊情况之球门区^^
		{
			double the_x=FRIGHT;
			if(fabs(g->opp[0].pos.x-FRIGHT)<4)
				the_x=g->opp[0].pos.x-1.5;
			if(predictball.x > the_x)//好可惜，球被守门员扑出，又反弹了^^
			{
				predictball.x -= ballspeed.x ;	
				predictball.y -= ballspeed.y ;

				ballspeed.x *=-SPEED_NORMAL;	
				ballspeed.y *= SPEED_TANGENT;

				predictball.x += ballspeed.x ;
				predictball.y += ballspeed.y ;
			}
		}
		else if( predictball.x < FLEFT && fabs(predictball.y-41.7)>10 ||  predictball.x < FLEFT+1.5 && fabs(predictball.y-41.7)<=10 )//球在我方门线区了^^
		{
			predictball.x -= ballspeed.x ;
			predictball.y -= ballspeed.y ;

			ballspeed.x *=-SPEED_NORMAL;	
			ballspeed.y *= SPEED_TANGENT;

			predictball.x += ballspeed.x ;
			predictball.y += ballspeed.y ;

		}
		else if( predictball.y < FBOT )//球在下底线^^
		{
			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			ballspeed.x *= SPEED_TANGENT;	//x是正弹了^^
			ballspeed.y *=-SPEED_NORMAL;	//y就是反弹^^

			predictball.x += ballspeed.x ;
			predictball.y += ballspeed.y ;

		}
		else if( predictball.y > FTOP )//球在上底线^^
		{
			predictball.x -= ballspeed.x ;
			predictball.y -= ballspeed.y ;

			ballspeed.x *= SPEED_TANGENT;
			ballspeed.y *=-SPEED_NORMAL;

			predictball.x += ballspeed.x ;
			predictball.y += ballspeed.y ;
		}

		if( predictball.x + predictball.y > FRIGHT +FTOP -CORNER)
		{//右上//那个去掉了一个等边三角形的地方^^
			double vx,vy;	
			vy=0.7071*ballspeed.y + 0.7071*ballspeed.x;	
			vx=-0.7071*ballspeed.y + 0.7071*ballspeed.x;

			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			vx *= SPEED_TANGENT;
			vy *=-SPEED_NORMAL;

			ballspeed.y = 0.7071 * vy - 0.7071 * vx;	
			ballspeed.x = 0.7071 * vy + 0.7071 * vx;

			predictball.x += ballspeed.x ;
			predictball.y += ballspeed.y ;

		}
		else if( predictball.x + predictball.y < FLEFT +FBOT+CORNER)
		{//左下
			double vx,vy;	
			vy=0.7071*ballspeed.y + 0.7071*ballspeed.x;	
			vx=-0.7071*ballspeed.y + 0.7071*ballspeed.x;

			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			vx *= SPEED_TANGENT;
			vy *=-SPEED_NORMAL;

			ballspeed.y = 0.7071 * vy - 0.7071 * vx;	
			ballspeed.x = 0.7071 * vy + 0.7071 * vx;

			predictball.x += ballspeed.x ;	
			predictball.y += ballspeed.y ;	
		}
		else if( predictball.x - predictball.y > FRIGHT -FBOT -CORNER)
		{//右下
			double vx,vy;	
			vy=0.7071*ballspeed.y - 0.7071*ballspeed.x;	
			vx=0.7071*ballspeed.y + 0.7071*ballspeed.x;

			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			vx *= SPEED_TANGENT;	
			vy *=-SPEED_NORMAL;

			ballspeed.y = 0.7071 * vy + 0.7071 * vx;	
			ballspeed.x = -0.7071 * vy + 0.7071 * vx;

			predictball.x += ballspeed.x ;	
			predictball.y += ballspeed.y ;			
		}
		else if( predictball.y - predictball.x > FTOP - FLEFT-CORNER)
		{//左上
			double vx,vy;	
			vy=0.7071*ballspeed.y - 0.7071*ballspeed.x;	
			vx=0.7071*ballspeed.y + 0.7071*ballspeed.x;

			predictball.x -= ballspeed.x ;	
			predictball.y -= ballspeed.y ;

			vx *= SPEED_TANGENT;	
			vy *=-SPEED_NORMAL;

			ballspeed.y = 0.7071 * vy + 0.7071 * vx;	
			ballspeed.x = -0.7071 * vy + 0.7071 * vx;

			predictball.x += ballspeed.x ;	
			predictball.y += ballspeed.y ;			
		}

	}

	g->ball.prepos[steps].x = predictball.x ;
	g->ball.prepos[steps].y = predictball.y ;
	g->ball.prepos[steps].z = Atan( ballspeed.y ,ballspeed.x );
}


//预测经过 1 个周期之后人的位置
Vector3D PredictMan(Game *g,int robot)
{
	//	double v=VelocityOne(g->robot[robot].speed,-125,125);
	double x=g->robot[robot].pos.x+1*(g->robot[robot].pos.x-g->robot[robot].oldpos.x);
	double y=g->robot[robot].pos.y+1*(g->robot[robot].pos.y-g->robot[robot].oldpos.y);
	Cut(x,FTOP,FBOT);
	Cut(y,FLEFT,FRIGHT);
	return Vect(x,y,0);
}
//预测经过k个周期之后人的位置  敌人
Vector3D PredictMan(Game *g,int robot,int k)
{
	//	double v=VelocityOne(g->robot[robot].speed,-125,125);
	double x=g->opp[robot].pos.x+k*(g->opp[robot].pos.x-g->opp[robot].oldpos.x);
	double y=g->opp[robot].pos.y+k*(g->opp[robot].pos.y-g->opp[robot].oldpos.y);
	Cut(x,FTOP,FBOT);
	Cut(y,FLEFT,FRIGHT);
	return Vect(x,y,0);
}


////////////让robot把球踢到ToPos的位置
void Kick(Game *g , int  robot , Vector3D ToPos )
{
	Kick(g,robot,g->ball.pos,ToPos);
}


//让robot把球从frompos踢到ToPos的位置
void Kick(Game *g , int  robot , Vector3D FromPos , Vector3D ToPos , double X)
{
	Vector3D ball = FromPos;		//这个球的位置不保险!!!

	Speed ToBall;		//人和球的相对位置
	ToBall = ball - g->robot[robot].pos ;

	Speed BallToGate ;		//球和球门的相对位置
	BallToGate = ToPos - ball ;

	double RunAngle ;
	RunAngle = ToBall.z - BallToGate.z;
	NormalAngle(RunAngle);

	//	if(X && fabs(RunAngle)<80 && g->robot[robot].NextToBall.dis<3.0 )

	if(fabs(RunAngle)<80 && g->robot[robot].NextToBall.dis<X && (g->HisSmallDis>5 || g->ball.bound==0))
	{
		if(RunAngle<0)
			Velocity(g,robot,-125,125);
		else
			Velocity(g,robot,125,-125);
		return;
	}



	double xishu=0.5;
	if(ToBall.dis>20)
		xishu=0.2;
	else
		xishu=(ToBall.dis-4)*(0.2-0.6)/20+0.6;

	RunAngle = ToBall.z + RunAngle * xishu;	// 可以调整  2 
	NormalAngle(RunAngle);
	Run_Angle(g,robot,RunAngle,125);
}


////////////让robot把球踢到ToPos的位置
void Kick2(Game *g , int  robot , Vector3D ToPos,double v)
{
	Kick(g,robot,g->ball.pos,ToPos,v);
}


//让robot把球从frompos踢到ToPos的位置
void Kick2(Game *g , int  robot , Vector3D FromPos , Vector3D ToPos , double v,double X)
{
	Vector3D ball = FromPos;		//这个球的位置不保险!!!

	Speed ToBall;		//人和球的相对位置
	ToBall = ball - g->robot[robot].pos ;

	Speed BallToGate ;		//球和球门的相对位置
	BallToGate = ToPos - ball ;

	double RunAngle ;
	RunAngle = ToBall.z - BallToGate.z;
	NormalAngle(RunAngle);

	//	if(X && fabs(RunAngle)<80 && g->robot[robot].NextToBall.dis<3.0 )

	if(fabs(RunAngle)<80 && g->robot[robot].NextToBall.dis<X && (g->HisSmallDis>5 || g->ball.bound==0))
	{
		if(RunAngle<0)
			Velocity(g,robot,-125,125);
		else
			Velocity(g,robot,125,-125);
		return;
	}



	double xishu=0.5;
	if(ToBall.dis>20)
		xishu=0.2;
	else
		xishu=(ToBall.dis-4)*(0.2-0.6)/20+0.6;

	RunAngle = ToBall.z + RunAngle * xishu;	// 可以调整  2 
	NormalAngle(RunAngle);
	Run_Angle(g,robot,RunAngle,v);
}


////后面的防守队员
void soldier1(Game *g,int robot,double the_x)
{

	Vector3D ball=g->robot[robot].BestBall;
	FromTo to_ball=ball-g->robot[robot].pos;
	//	double y=meet_y(ball,Vect(6,41.7,0),20);


	if(ball.x>50)
		Position(g,robot,Vect(20,41.7,0),to_ball.z,2);
	else
		push_out(g,robot);
}

////哈哈，这些都是士兵！！！！！ 前面的
void soldier2(Game *g,int robot,double the_x)
{
	///  球在他手里，并且球不在左边，没有对方队员的威胁
	if( fabs(g->robot[robot].pos.y-41.7)<10 && g->robot[robot].ToBall.dis<4.2 && g->ball.pos.x > g->robot[robot].pos.x-1.5)
	{
		int i=1;
		if(fabs(g->ball.pos.y-41.7)<fabs(g->robot[robot].pos.y-41.7)-1.5)//球更靠里！！
			i*=-1;
		if(g->ball.pos.y<41.7)//机器人在下面翻过来
			i*=-1;
		Velocity(g,robot,i*125,-i*125);// dxp极速转动
		return;
	}

	if(g->ball.pos.x>the_x-1.4)//球在右边
	{
		Vector3D pt;
		pt.x=the_x;
		pt.y=g->ball.pos.y;
		if(g->ball.speed.x<0)//球在向左跑
		{
			double y2=g->ball.pos.y+(the_x+1.5-g->ball.pos.x)*g->ball.speed.y/g->ball.speed.x;
			if(y2>FTOP)
				y2=FTOP;
			else if(y2<FBOT)
				y2=FBOT;

			pt.y=(pt.y+y2)/2;
		}
		Position(g,robot,pt,90,1);//跑位
	}
	else
		push_out(g,robot);

}


////包抄功臣！！！！！！
void wait(Game *g,int robot)
{
	FromTo to_opp,to_pos=g->waitpos-g->robot[robot].pos;
	for(int i=0;i<=4;i++)
	{
		to_opp=g->opp[i].pos-g->robot[robot].pos;
		if(to_opp.dis<5)
		{
			if(to_opp.z-to_pos.z>0 && to_opp.z-to_pos.z<90)//往下跑
			{
				Run_Angle(g,robot,to_opp.z-92,125);
				return;
			}
			else if(to_opp.z-to_pos.z<0 && to_opp.z-to_pos.z>-90)
			{
				Run_Angle(g,robot,to_opp.z+920,125);
				return;
			}
		}
	}
	Position(g,robot,g->waitpos,g->waitpos.z);	
}



//此函数用于防守，把球搞出来
void push_out(Game *g,int robot){

	Vector3D ball=g->robot[robot].BestBall;

	if(robot!=g->MySmallDisMan && g->MySmallDis<4 && g->ball.bound!=0 && g->MySmallDis<4)
	{
		double angle=g->ball.ToGate.z-g->robot[robot].ToBall.z;
		NormalAngle(angle);
		if(fabs(angle)<40)
			Kick(g,robot,g->robot[g->MySmallDisMan].pos,g->BestGate,false);
		else
			Kick(g,robot,ball,g->BestGate,false);
	}


	Kick(g,robot,ball,g->BestGate);

}

//如果可以直接射门  就是球可以打进
bool good_pos(Game *g,int robot,Vector3D &ball,double limit)
{
	FromTo robot_ball=ball-g->robot[robot].pos;					//机器人 到 球 的角度
	FromTo agl1=Vect(FRIGHT,GTOP,0)-g->robot[robot].pos;	//机器人 到 上门柱 的角度
	FromTo agl2=Vect(FRIGHT,GBOT,0)-g->robot[robot].pos;	//机器人 到 下门柱 的角度

	double agl=(agl1.z-agl2.z)*limit;

	if(robot_ball.z > agl1.z + agl || robot_ball.z < agl2.z - agl) //不在第一范围之内
		return false;

	FromTo ball_door1 = Vect(FRIGHT,GTOP,0)-ball;			//上门柱
	if(ball_door1.z < agl1.z - agl)
		return false;

	FromTo ball_door2 = Vect(FRIGHT,GBOT,0)-ball;			//下门柱
	if(ball_door2.z > agl2.z + agl)
		return false;

	return true;
}



//最好的击球点！！！，，，，  距离方法
Vector3D best_ball(Game *g,int robot)
{
	double v=sqrt(g->robot[robot].ToBall.dis);
	if(v>1.3)
		v=1.3;
	if(g->robot[robot].speed.v>v)
		v=g->robot[robot].speed.v;


	int steps1=toint((g->robot[robot].ToBall.dis-4)/v);//计算碰现在球需要的步数

	if(steps1<=2)
	{
		PredictBall(g,steps1);
		return g->ball.prepos[steps1];
	}


	PredictBall(g,steps1/2);

	int steps2=toint((Distance(g->robot[robot].pos,g->ball.prepos[steps1/2])-4)/v);//计算碰现在球需要的步数


	PredictBall(g,steps1/2+steps2/2);


	return g->ball.prepos[steps1/2+steps2/2];


}

/*
角度    k值
0       0.6
70      1
140     1.5


*/
//最好的击球点！！！，，，，  距离方法
Vector3D best_ball1(Game *g,int robot)
{
	double v=sqrt(fabs(g->robot[robot].ToBall.dis-4));
	if(v>1.1)
		v=1.1;
	if(g->robot[robot].speed.v>v)
		v=g->robot[robot].speed.v;

	long steps=toint((g->robot[robot].ToBall.dis-4)/v);//计算碰现在球需要的步数
	PredictBall(g,steps);

	double angle_to_me=180+g->robot[robot].ToBall.z;
	NormalAngle(angle_to_me);
	double angle=g->ball.speed.z-angle_to_me;
	NormalAngle(angle);
	angle=fabs(angle);//dxp计算角度

	double k=0.7+angle*(1-0.7)/80;

	if(k>1)k=1;

	steps=toint(k*steps);
	steps=Cut(steps,0,20);
	PredictBall(g,steps);

	return g->ball.prepos[steps];


}



//最好的击球点！！！，，，， 角平分线渐进算法
Vector3D best_ball2(Game *g,int robot)
{

	double agl1,agl2,agl0,agl_ball;//这是角度的两个范围
	agl2=g->robot[robot].ToBall.z;

	if(g->ball.speed.v<0.1)//球的速度太慢
	{
		return g->ball.pos;
	} 

	agl_ball=Atan(g->ball.speed.y,g->ball.speed.x);
	agl0=agl2-agl_ball;////差值
	NormalAngle(agl0);	

	if(fabs(agl0)<1)			///////球太正
		return g->ball.pos;


	if(agl0>90)
		agl1=agl2-90;
	//		agl1=agl_ball+90;
	else
		if(agl0<-90)
			agl1=agl2+90;
		else
		{
			agl1=agl_ball;
			if(agl1-agl2>180)
				agl2+=360;
			if(agl2-agl1>180)
				agl1+=360;
		}


		double sin1=fabs(sin((g->robot[robot].ToBall.z-agl_ball)*3.1415/180.));
		double sin2;


		////1.3就算是小球的速度吧 k为速度比
		double robot_v=0.2;
		if(g->robot[robot].speed.v>robot_v)
			robot_v=g->robot[robot].speed.v;

		double k=robot_v/g->ball.speed.v;

		double avg;
		int i=0;

		// 	if(robot==3)
		// 		fprintf(g->file,"比值：k=%4.2f;中间角:%4.2f；侧角:%4.2f;",k,agl2,agl1);

		while(i++<10)
		{
			avg=(agl1+agl2)/2;
			sin2=fabs(sin(g->robot[robot].ToBall.z-avg));
			if(sin2/sin1>k)   //////离g->robot[i].ToBall.z  太近了
				agl2=avg;
			else
				agl1=avg;
		}

		//	fprintf(g->debugfile,"  最后角度：agl1=%f\n",avg);


		double k1=tan(avg*3.1415/180);//这个是我们需要的角度

		double k2,x1,y1,x2,y2,x,y;//下面是求两条直线的交点
		k2=g->ball.speed.y/g->ball.speed.x;
		x1=g->robot[robot].pos.x;
		y1=g->robot[robot].pos.y;

		x2=g->ball.pos.x;
		y2=g->ball.pos.y;

		x=(k2*x2-k1*x1+y1-y2)/(k2-k1);
		y=k1*(x-x1)+y1;


		//	fprintf(g->debugfile,"4号：(%f,%f)------小球：(%f,%f)   结果：(%f,%f)  k=%f;角度：%f,%f\n",g->robot[4].pos.x,g->robot[4].pos.y,g->ball.speed.x,g->ball.speed.y,x,y,k,agl1,agl2);



		return Vect(x,y,0);
}

//	角色转换函数！！！
void Change(Game *g)
{	//为什么只和3号球员交换呢？^^
	if(g->robot[g->r[1]].TrueDis<g->robot[g->r[3]].TrueDis-0.5 )//1号球员比3号球员近^^
		Change(g,1,3);

	if(g->robot[g->r[2]].TrueDis<g->robot[g->r[3]].TrueDis-0.5 )
		Change(g,2,3);

	if(g->robot[g->r[4]].TrueDis<g->robot[g->r[3]].TrueDis-0.5)
		Change(g,4,3);

	g->r[0] = 0;


	if(g->ball.pos.x>40)
	{

		Vector3D v2={70,g->ball.pos.y,0};
		Vector3D v4=g->waitpos;

		int most_far2,most_far4;				///和哪个点最远
		double most_far_value2,most_far_value4;	///最远距离


		most_far2=1;
		most_far_value2=Distance(g->robot[g->r[2]].pos,v2);

		if(Distance(g->robot[g->r[2]].pos,v4)>most_far_value2)
		{
			most_far2=2;
			most_far_value2=Distance(g->robot[g->r[2]].pos,v4);
		}


		most_far4=1;
		most_far_value4=Distance(g->robot[g->r[4]].pos,v2);

		if(Distance(g->robot[g->r[4]].pos,v4)>most_far_value4)
		{
			most_far4=2;
			most_far_value4=Distance(g->robot[g->r[4]].pos,v4);
		}


		//	4号 更远  需要交换
		if(most_far_value4>most_far_value2 && most_far4==2)/////4号机器人到4号点的距离>4号机器人到2号点的距离&&4号机器人到4号点的距离>2号机器人到任意点的距离^^
			Change(g,2,4);
		if(most_far_value4<most_far_value2 && most_far2==1)/////2号机器人到2号点的距离>4号机器人到任意点的距离^^
			Change(g,2,4);
	}

}


//转变角色
void Change(Game *g,int r1,int r2)
{
	int r=g->r[r1];
	g->r[r1]=g->r[r2];
	g->r[r2]=r;

}

//察看是否胜利，有进球动作的
bool win(Game *g)
{

	if(g->ball.pos.x < FRIGHT)//还没进球了^^
		return false;

	for(int i=1;i<5;i++)//守门员就不欢呼了哈^^
	{
		if(g->robot[i].pos.x >FRIGHT-10)
			Position(g,i,Vect(FRIGHT-13,g->robot[i].pos.y,0),0,1);//Vect(FRIGHT-11^^
		else
			Velocity(g,i,125,-30);
	}
	return true;
}
//角色复位
void reset(Game *g)
{
	int i;
	for(i=0;i<5;i++)
		g->r[i]=i;
}


//避免犯规的函数
void AvoidFouls1(Game *g, int state)
{
	//处理大禁区的球员总数犯规
	int InLRestricted = 0;
	int InRestrictedFlag = 0;
	for (int i = 0; i < 5; i++)
	{
		if (IsInLRestricted(g->robot[i].pos))
		{
			InLRestricted++;
			InRestrictedFlag |= (1 << i);  //用此数的2进制表示那个球员在大禁区
		}
	}
	int State = 0, num = 0;
	for (int i = 0; i < 5; i++)
	{
		int id = -1, ans = INT_MAX, angle = -1;
		for (int j = 0; j < 5; j++)
		{
			if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)  //
			{
				if (g->robot[j].pos.x - FLEFT > 3 * RR)  //球在我方禁区
				{
					int len = INT_MAX;
					len = LRIGHT - g->robot[j].pos.x;
					if (len < ans)
					{
						id = j, angle = 0;
						ans = len;
					}
					len = LTOP - g->robot[j].pos.y;
					if (len < ans)
					{
						id = j, angle = 90;
						ans = len;
					}
					len = g->robot[j].pos.y - LBOT;
					if (len < ans)
					{
						id = j, angle = -90;
						ans = len;
					}
				}
			}
		}
		if (id != -1)
		{
			num++;
			if (num > InLRestricted - 3)
			{
				break;
			}
			Run_Angle(g, id, angle, 125);
			State |= (1 << id);
		}
	}
	InLRestricted -= num;
	if (state == 0)  //无
	{
		for (int i = 0; i < 5; i++)
		{
			int id = -1, ans = INT_MAX, angle = -1;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					int len = INT_MAX;
					len = LRIGHT - g->robot[j].pos.x;
					if (len < ans)
					{
						id = j, angle = 0;
						ans = len;
					}
					len = LTOP - g->robot[j].pos.y;
					if (len < ans)
					{
						id = j, angle = 90;
						ans = len;
					}
					len = g->robot[j].pos.y - LBOT;
					if (len < ans)
					{
						id = j, angle = -90;
						ans = len;
					}
				}
			}
			if (id != -1)
			{
				num++;
				if (num > InLRestricted - 3)
				{
					break;
				}
				Run_Angle(g, id, angle, 125);
				State |= (1 << id);
			}
		}
	}
	else if (state == 2)
	{
		for (int i = 0; i < InLRestricted - 3; i++)
		{
			int id = -1, ans = FTOP;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					if (g->robot[j].pos.y < ans)
					{
						id = j;
						ans = g->robot[i].pos.y;
					}
				}
			}
			if (id != -1)
			{
				Run_Angle(g, id, -90, 125);
				State |= (1 << id);
			}
		}
	}
	else if (state == 3)
	{
		for (int i = 0; i < InLRestricted - 3; i++)
		{
			int id = -1, ans = FBOT;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					if (g->robot[j].pos.y > ans)
					{
						id = j;
						ans = g->robot[i].pos.y;
					}
				}
			}
			if (id != -1)
			{
				Run_Angle(g, id, 90, 125);
				State |= (1 << id);
			}
		}
	}

	//处理小禁区的球员总数犯规
	int InSRestricted = 0;
	InRestrictedFlag = 0;
	for (int i = 0; i < 5; i++)
	{
		if (IsInSRestricted(g->robot[i].pos))
		{
			InSRestricted++;
			InRestrictedFlag |= (1 << i);
		}
	}
	State = 0;
	if (state == 0)
	{
		for (int i = 0; i < InSRestricted - 1; i++)
		{
			int id = -1, ans = FLEFT;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					if (g->robot[j].pos.x > ans)
					{
						id = j;
						ans = g->robot[i].pos.x;
					}
				}
			}
			if (id != -1)
			{
				Run_Angle(g, id, 0, 125);
				State |= (1 << id);
			}
		}
	}
	else if (state == 2)
	{
		for (int i = 0; i < InSRestricted - 1; i++)
		{
			int id = -1, ans = FTOP;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					if (g->robot[j].pos.y < ans)
					{
						id = j;
						ans = g->robot[i].pos.y;
					}
				}
			}
			if (id != -1)
			{
				Run_Angle(g, id, 0, 125);
				State |= (1 << id);
			}
		}
	}
	else if (state == 3)
	{
		for (int i = 0; i < InSRestricted - 1; i++)
		{
			int id = -1, ans = FBOT;
			for (int j = 0; j < 5; j++)
			{
				if ((State & (1 << j)) == 0 && (InRestrictedFlag & (1 << j)) != 0)
				{
					if (g->robot[j].pos.y > ans)
					{
						id = j;
						ans = g->robot[i].pos.y;
					}
				}
			}
			if (id != -1)
			{
				Run_Angle(g, id, 0, 125);
				State |= (1 << id);
			}
		}
	}
}


bool AvoidFouls(Game *g,int state)   //xmm
{
	bool flag=false;
	int i,j;
	Vector3D op;
	op.x=48;op.y=43;op.z=0;
	int num=0;
	bool robot[5]={0,0,0,0,0};
	for(i=0;i<5;i++)//看对方5个机器人中有几个在我方小禁区，在的robot为 ture
	{
		for(j=0;j<=2;j++)
		{
			if( !IsInSRestricted(PredictMan(g,i,j)))//预测机器人j步以后位置的点， 判断点是否在本方小禁区内部
			{
				break;
			}
		}
		if(j==3)//若当时和后两步都在小禁区，则说明对方的此机器人在小禁区
		{
			num++;
			robot[i]=true;
		}
	}

	if(num==1)
	{
		for(i=0;i<5;i++)// i机器人
		{
			if(robot[i]==false)
			{
				for(j=0;j<=4;j++)//接下来的后J步
				{
					if(IsInSRestricted(PredictMan(g,i,j)))//如果在，
					{
						::Through(g,i,op,125);//最快速度冲
						flag=true;
						break;
					}
				}
			}
		}
	}

	int num2=0;
	bool robot2[5]={0,0,0,0,0};

	for(i=0;i<5;i++)//己方机器人
	{
		if(IsInLRestricted(g->robot[i].pos))//判断点是否在本方大禁区内部
		{
			num2++;
			robot[i]=true; //（ 这里应该是robot2【】 ）
		}
	}
	if(num2==3)//人数一到3个就拖一个走
	{
		for(i=0;i<5;i++)
		{
			if(robot[i]==false)  //（ 这里应该是robot2【】 ）
			{
				for(j=0;j<=4;j++)
				{
					if(IsInLRestricted(PredictMan(g,i,j)))
					{
						::Through(g,i,op,125);
						flag=true;
						break;
					}
				}
			}
		}
	}
	return flag;
}
//防止球员进入球门

bool AvoidFouls2(Game *g,int state)   //dxp
{
	bool flag=false;
	int i,j;
	Vector3D op; //跑点位置
	int num=0;//统计己方有几个在大禁区里面
	bool robot[5]={0,0,0,0,0};

	for(i=1;i<5;i++)//己方机器人
	{
		if(IsInLRestricted(g->robot[i].pos))//判断点是否在本方大禁区内部
		{
			num++;
			robot[i]=true; //
		}
	}

	if( num>2 )  //大禁区人数过多，转移离球最远的机器人
	{
		double maxdis=-1;
		int maxdisrobot=-1;
		for( i=1;i<5;i++ )  //找到最远的
		{
			if( robot[i]==true )
			{
				double distance_to_ball = Distance( g->ball.pos , g->robot[i].pos )  ;
				if( distance_to_ball > maxdis )
				{
					maxdis=distance_to_ball;
					maxdisrobot=i;
				}
			}
		}

		if( g->ball.pos.y > ( GTOP+GBOT ) / 2 )
		{
			op.x =24.5;
			op.y =53.2;
			op.z =0 ;
			Through( g, maxdisrobot , op, 125 ) ;
		}
		else
		{
			op.x =24.5;
			op.y =30.2;
			op.z =0 ;
			Through( g, maxdisrobot , op, 125 ) ;
		}
		flag=1;
	}


	return flag;
}
//防止球员进入球门

bool SpecialSituation1(Game *g) //进入己方底线互相推球后的一个情况
{
	int i,j;
	int sum=0; //统计己方底线推球的非守门员个数

	if( g->ball.pos.x - FLEFT <= 2*RR  &&  (g->ball.pos.y>=GTOP - 3*BR   ||  g->ball.pos.y<=GBOT + 3*BR )     )
		return 1;

	if( g->ball.pos.x - FLEFT >=2*RR )  //机器人贴边是x=9.5左右
		return 0;

	for( i=1;i<5;i++ ) //统计有几个队员是靠着底线且互相贴身的
	{
		if(  g->robot[i].pos.x <= 9.5+RR  )  
		{
			int flag=0;
			for( j=1;j<5;j++)
			{
				if( j!=i )
				{
					if(   fabs( g->robot[i].pos.y  -   g->robot[j].pos.y ) <=3*RR    )  
					{
						flag=1;
						break;
					}
				}
			}
			if( flag )
				sum++;
		}
	}

	if( sum>=2 )
		return 1;
	else
		return 0;
}
bool SpecialSituation2(Game *g)//判定球的位置，队员退出防守
	{
		int i,j;
		if(   (g->ball.pos.x <= FLEFT + 2*RR)   &&  ( g->ball.pos.y <= FTOP - 12   &&  g->ball.pos.y >=FBOT+12 )   )
			return 1;
		if(   (g->ball.pos.x <= FLEFT + 3*RR)   &&  ( g->ball.pos.y <= 50.6   &&  g->ball.pos.y >=27   )   )
			return 1;

		return 0;
	}




//防止球员进入球门
bool DefenseAvoidFouls( Game *g,int state ) //dxp
{
	int i , j ;
	//bool ballpos; // 0标记球在上方，1标记下方 
	bool hashrobot[5]; //标记此机器人是否已经分配
	for(i=1;i<5;i++)
	{
		hashrobot[i]=0;
	}

	Vector3D op[5]; //跑点位置
	op[0].x =22; op[0].y =45; op[0].z =0 ;//右边上 
	op[1].x =22; op[1].y =38; op[1].z =0 ;//右边下
	op[2].x =14; op[2].y =62; op[2].z =0 ;//上方
	op[3].x =14; op[3].y =21; op[3].z =0 ;//下方
	op[4].x =22; op[4].y =48; op[4].z =0 ;//右中

	for( j=0;j<4;j++ )  //每次为位置0到3找一个机器人去,如果会影响到球的走位则排除此机器人
	{
		double mindis=10000;
		int minrobot=-1;
		double distance_to_op;

		for(i=1;i<5;i++)//4个机器人
		{
			if( hashrobot[i]==true )
				continue;		
			if(   ( g->ball.pos.y >= op[j].y  &&  g->ball.pos.y <= g->robot[i].pos.y)    || 
				  ( g->ball.pos.y <= op[j].y  &&  g->ball.pos.y >= g->robot[i].pos.y)    ) //如果会影响球，排除此机器人
			{
				if(  (g->ball.pos.x < op[j].x-RR)   &&   (g->ball.pos.x < g->robot[i].pos.x - RR)   ) ;
				else
					continue;
			}
			distance_to_op = Distance( g->robot[i].pos, op[j] ) ;
			if( distance_to_op < mindis ) 
			{
				minrobot=i;
				mindis=distance_to_op;
			}
		}

		if( minrobot != -1 )//去自己的位置
		{
			///处理路上被敌方机器人挡住
			if( g->robot[minrobot].pos.y > op[j].y  ) //在往下走被挡的情况
			{
				bool rflag=0;
				for( i=0;i<5;i++ )
				{
					if(   (g->opp[i].pos.x > g->robot[minrobot].pos.x - 2*RR)   &&  (g->opp[i].pos.x < g->robot[minrobot].pos.x + 2*RR) 
						&&  (g->opp[i].pos.y < g->robot[minrobot].pos.y  )   &&  (g->opp[i].pos.y > g->robot[minrobot].pos.y - 3*RR)     )
					{
						rflag=1;
						break;
					}
				}
				if( rflag==1 )
				{
					Vector3D  op_1;
					op_1.x =  g->robot[minrobot].pos.x + 3*RR ;
					op_1.y =  g->robot[minrobot].pos.y ;
					op_1.z =  g->robot[minrobot].pos.z ;
					Through( g , minrobot , op_1 , 125 ) ;
					hashrobot[minrobot]=1;
				}
			}
			else if  ( g->robot[minrobot].pos.y < op[j].y  ) //在机器人往上走被挡的情况
			{
				bool rflag=0;
				for( i=0;i<5;i++ )
				{
					if(   (g->opp[i].pos.x > g->robot[minrobot].pos.x - 2*RR)   &&  (g->opp[i].pos.x < g->robot[minrobot].pos.x + 2*RR) 
						&&  (g->opp[i].pos.y > g->robot[minrobot].pos.y  )   &&  (g->opp[i].pos.y < g->robot[minrobot].pos.y + 3*RR)     )
					{
						rflag=1;
						break;
					}
				}
				if( rflag==1 )
				{
					Vector3D  op_1;
					op_1.x =  g->robot[minrobot].pos.x + 3*RR ;
					op_1.y =  g->robot[minrobot].pos.y ;
					op_1.z =  g->robot[minrobot].pos.z ;
					Through( g , minrobot , op_1 , 125 ) ;
					hashrobot[minrobot]=1;
				}
			}
			////////////

			Through( g , minrobot , op[j] , 125 ) ;//若没有被阻挡，则去目标地点
			hashrobot[minrobot]=1;
		}
	}

	for( i=1;i<5;i++)
	{
		if( hashrobot[i]!=true )//因为各种情况没分配的
		{
			Through( g,i,op[4],125 );
		}
	}

	return 0;

}
void GoalieSpecialSituation1(Game *g ) 
{
	Vector3D go;
	if(  g->ball.pos.y<= GBOT+3*BR   )   
	{
		go.x =6.50432;
		go.y =36.0000;
		go.z =64.000;
		GoaliePosition(g,0,go,60,0.2);
	}
	else if (  g->ball.pos.y>= GTOP-3*BR  )   
	{
		go.x =6.30432;
		go.y =47.790;
		go.z =128.000;
		GoaliePosition(g,0,go,128,0.2);
	}

	return ;
}


void AvoidInDoor(Game *g, int id)
{
	double angle, len;
	double curangle = g->robot[id].pos.z;
	while (curangle < -90)	curangle += 90;
	while (curangle >= 0)	curangle -= 90;
	angle = curangle + 90;
	if (angle > 45)
	{
		angle = 90 - angle;
	}
	angle = angle + 45;
	len = 1.4142135 * RR * sin(angle * PI / 180);
	if (g->robot[id].pos.x < FLEFT + len)
	{
		Run_Angle(g, id, 0, 125);
	}
	else if (g->robot[id].pos.x > FRIGHT - len)
	{
		Run_Angle(g, id, 180, 125);
	}
}

//防止球员撞墙，绕不出来
void GoBack(Game *g)
{
	int i;
	//	Vector3D pos;
	for(i = 1; i < 5; i ++)
	{
		//底线 停滞不前 转不起来
		if((g->robot[i].pos.x - FLEFT) <= BR && g->robot[i].pos.z >= 175)
		{
			//pos.x = g->robot[i].pos.x + 3;
			//pos.y = g->robot[i].pos.y - 3;
			//Position(g,i,pos,120);
			Run_Angle(g,i,-65,125);
			return ;
		}
		else if((g->robot[i].pos.x - FLEFT) <= BR && g->robot[i].pos.z <= -175)
		{
			//pos.x = g->robot[i].pos.x + 3;
			//pos.y = g->robot[i].pos.y + 3;
			//Position(g,i,pos,-120);
			Run_Angle(g,i,60,125);
		}
		else if(g->robot[i].pos.x >= (FRIGHT - BR) && g->robot[i].pos.z <= 5)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y - 3;
			//Position(g,i,pos,-60);
			Run_Angle(g,i,120,125);
		}
		else if(g->robot[i].pos.x >= (FRIGHT - BR) && g->robot[i].pos.z <= -5)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y + 3;
			//Position(g,i,pos,60);
			Run_Angle(g,i,-120,125);
		}
		//边线
		else if((g->robot[i].pos.y - FBOT) <= BR && g->robot[i].pos.z <= -80)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y + 3;
			//Position(g,i,pos,-30);
			Run_Angle(g,i,120,125);
		}
		else if((g->robot[i].pos.y - FTOP) >= BR && g->robot[i].pos.z >= 80)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y - 3;
			//Position(g,i,pos,30);
			Run_Angle(g,i,-120,125);
		}
		//拐角
		else if((fabs(g->robot[i].pos.x - FLEFT) <= 4.5 && fabs(g->robot[i].pos.y - FBOT) <= 6) && g->robot[i].pos.z <= -120)
		{
			//pos.x = g->robot[i].pos.x + 3;
			//pos.y = g->robot[i].pos.y + 3;
			//Position(g,i,pos,30);
			Run_Angle(g,i,60,125);
		}
		else if((fabs(g->robot[i].pos.x - FRIGHT) <= 6 && fabs(g->robot[i].pos.y - FBOT) <= 4.5) && g->robot[i].pos.z <= -50)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y + 3;
			//Position(g,i,pos,30);
			Run_Angle(g,i,120,125);
		}
		else if((fabs(g->robot[i].pos.x - FLEFT) <= 4.5 && fabs(g->robot[i].pos.y - FTOP) <= 6) && g->robot[i].pos.z >= 120)
		{
			//pos.x = g->robot[i].pos.x + 3;
			//pos.y = g->robot[i].pos.y - 3;
			//Position(g,i,pos,30);
			Run_Angle(g,i,-60,125);
		}
		else if((fabs(g->robot[i].pos.x - FRIGHT) <= 6 && fabs(g->robot[i].pos.y - FTOP) <= 4.5) && g->robot[i].pos.z >= 50)
		{
			//pos.x = g->robot[i].pos.x - 3;
			//pos.y = g->robot[i].pos.y - 3;
			//Position(g,i,pos,30);
			Run_Angle(g,i,-120,125);
		}
	}
}	


//处理球员转弯
void ThroughCorner(Game *g, int id)
{
	if((fabs(g->robot[id].pos.x - FLEFT) < 4.5 && fabs(g->robot[id].pos.y - FTOP) < 6) || (fabs(g->robot[id].pos.x - FRIGHT) < 6 && fabs(g->robot[id].pos.y - FBOT) < 4.5))
	{
		Run_Angle(g, id, 60, 125);
	}
	if((fabs(g->robot[id].pos.x - FLEFT) < 4.5 && fabs(g->robot[id].pos.y - FBOT) < 6) || (fabs(g->robot[id].pos.x - FRIGHT) < 6 && fabs(g->robot[id].pos.y - FTOP) < 4.5))
	{ 
		Run_Angle(g, id, -60, 125);
	}
}

//球被撞开进入一定区域后，布置最后的防守
void LastDefense(Game *g)
{
	int defenseID = -1;
	double lmin = INT_MAX;
	for (int i = 0; i < 5; i++)//dxp求出我方离球最近的机器人
	{
		double len = Distance(g->ball.pos, g->robot[i].pos);
		if (defenseID == -1 || len < lmin)
		{
			defenseID = i;
			lmin = len;
		}
	}
	for (int i = 0; i < 5; i++)
	{
		if (i != defenseID)
		{
			Run_Angle(g, i, 0, 125);
		}
	}
	if (state(g->ball.pos) == 2)//state预判防守哪里，2为底线下方
	{
		Vector3D Pos;
		Pos.x = FLEFT * RR * 1.4142135;
		Pos.y = STOP - 1;
		Pos.z = 135;
		if (g->ball.pos.y > STOP)
		{
			Position(g, defenseID, Pos, Pos.z, 1);
		}
		else
		{
			Run_Angle(g, defenseID, Pos.z, 125);
		}
	}
	else if (state(g->ball.pos) == 3)//防守底线下方
	{
		Vector3D Pos;
		Pos.x = FLEFT * RR * 1.4142135;
		Pos.y = SBOT + 1;
		Pos.z = -135;
		if (g->ball.pos.y < SBOT)
		{
			Position(g, defenseID, Pos, Pos.z, 1);
		}
		else
		{
			Run_Angle(g, defenseID, Pos.z, 125);
		}
	}
}

void attackpaint(Game *g)//禁区进攻设定 优先于pushball有两个人以上 触发！！！
{
		if(!(g->ball.pos.y<=LTOP&&g->ball.pos.y>=LBOT&&g->ball.pos.x>= FRIGHT-(LRIGHT-FLEFT)))//球莫有进入对方大禁区啊。。。好吧。。。
		{
			return   ;//同学你不归我管。
		}


	//大禁区有多少队员！！！！
	int sum =0;
	int id =0;
	for(int i=1;i<5;i++)
	{
		if(g->robot[i].pos.y<=LTOP+2&&g->robot[i].pos.y>=LBOT-2&&g->robot[i].pos.x>= FRIGHT-(LRIGHT-FLEFT))
			sum++;
		else
			id =i;

	}
	if(sum<2)
	{
		return ;//还不到时机

	}

	if(sum>=3)//不好！太多人了 ，那位同学别进来了
	{
		//pos.x=(FRIGHT+FLEFT)/2;
		//pos.z = -180;
		Angle(g,id,g->robot[id].pos.z+90);//等待 等球出来或者别人出来~
	}
	//处理犯规

	//你们大禁区有几个人呢？
	int oppon = 0;

	for(int i=0;i<5;i++)
	{
		if(g->opp[i].pos.y<=LTOP+2 &&g->opp[i].pos.y>=LBOT -2 &&g->opp[i].pos.x>= FRIGHT-(LRIGHT-FLEFT))
			oppon++;
	}

	if(oppon<=3)//你们没犯规 好吧，优化下我们的队员！！！！
	{
		//守门员应该在小禁区吧。。。
		int oppid=0;
		for(int i=0;i<5;i++)
		{
			if(g->opp[i].pos.y<=STOP+2&&g->opp[i].pos.y>=SBOT-2&&g->opp[i].pos.x>= FRIGHT-(SRIGHT-FLEFT))//小样你是守门员吧，别忽悠我哦。
			{
				oppid=i;
				break;
			}

		}
		int noid=0;//不参考的id
		for(int i=1;i<5;i++)//对面守门员太恶心，先让一下,哎，你们谁是守门员啊！
		{
			if(Distance(g->opp[oppid].pos,g->robot[i].pos)<=2*(RR+BR)+5)//和守门员的距离还是蛮小的时候
			{
				Angle(g,id,g->robot[i].pos.z+90);//转死你！！！！！
				noid = i;
				break;
				//只设置一个队员
			}
		}

		//现在球离谁最近呢？
		int goalrobot ;
		if(noid==1)
			goalrobot =2;
		else
			goalrobot =1;
		double mindis=Distance(g->robot[goalrobot].pos,g->ball.pos);
		for(int i =1 ;i <5;i++)
		{
			if(i!=noid)
			if(Distance(g->robot[i].pos,g->ball.pos)<mindis)
			{
				mindis = Distance(g->robot[i].pos,g->ball.pos);
				goalrobot = i;
			}
		}
		int goalrobot2  =1;//查找离球第二远的某人
		double min=9999; 
		for(int i =1 ; i<5;i++)
		{
			if(i!=goalrobot&&i!=noid)
			{
				if(Distance(g->robot[i].pos,g->ball.pos)<min)
			{
				min= Distance(g->robot[i].pos,g->ball.pos);
				goalrobot2 = i;
			}

			}

		}

		//goalrobot 和 goalrobot2 配合射门

		if(Distance(g->opp[oppid].pos,g->robot[goalrobot].pos)<=2*(RR+BR)+5)//离守门员太近了
		{

			Kick(g,goalrobot,g->robot[goalrobot2].pos);//传球比较好吧，


		}
		else//射门！！！！
		{
				Kick(g,goalrobot,g->robot[goalrobot].BestBall,g->BestGate);

		}
		
			for(int i =1 ; i<5;i++)//设置没有设置过的队员的动作！！！！！！差点把你们忘了
			{
					if(g->robot[i].pos.y<=LTOP+2&&g->robot[i].pos.y>=LBOT-2&&g->robot[i].pos.x>= FRIGHT-(LRIGHT-FLEFT))
						Kick(g,i,g->robot[i].BestBall,g->BestGate);

			}

		

	}
			

}



void push_advanced(Game *g,int stat,int num)// 如果已经在边线或者底线推球的话 加强某些球员的动作！！！！
{
	if(stat==0)
		return ;

		int rob =1;
		if(g->ball.pos.y<=LTOP&&g->ball.pos.y>=LBOT&&g->ball.pos.x>= FRIGHT-(LRIGHT-FLEFT))//球进入大禁区
		{
			Vector3D  pos;
			pos.z=0;
			pos.x = FRIGHT-(LRIGHT-FLEFT);
			pos.y =(FTOP+FBOT)/2;
		    
            if(g->ball.pos.y<=STOP&&g->ball.pos.y>=SBOT&&g->ball.pos.x>= FRIGHT-(SRIGHT-FLEFT))//球在小禁区 ，射门！
				Through(g,rob,g->ball.pos);
			else
		       Through(g,rob,pos);//某某某队员去排眼
			
        
			//大禁区有多少队员！！！！
			int sum =0;
			int id =0;
			for(int i=1;i<5;i++)
			{
				if(g->robot[i].pos.y<=LTOP+2&&g->robot[i].pos.y>=LBOT-2&&g->robot[i].pos.x>= FRIGHT-(LRIGHT-FLEFT))
					sum++;
				else if(i!=rob)
					id =i;

			}
			if(sum>=3)//不好！太多人了 ，那位同学别进来了
			{
				pos.x=(FRIGHT+FLEFT)/2;
				pos.z = -180;
				if(id!=0)
                 Through(g,id,pos);
			}

			//你们大禁区有几个人呢？
			int oppon = 0;

			for(int i=0;i<5;i++)
			{
				if(g->opp[i].pos.y<=LTOP+2 &&g->opp[i].pos.y>=LBOT -2 &&g->opp[i].pos.x>= FRIGHT-(LRIGHT-FLEFT))
					oppon++;
			}
			if(oppon>3)//你们都犯规了 那我不管了。。。。。
			{
				return ;
			}
			//守门员应该在小禁区吧。。。
			int oppid=0;
			for(int i=0;i<5;i++)
			{
				if(g->opp[i].pos.y<=STOP+2&&g->opp[i].pos.y>=SBOT-2&&g->opp[i].pos.x>= FRIGHT-(SRIGHT-FLEFT))//小样你是守门员吧，别忽悠我哦。
				{
					oppid=i;
					break;
				}

			}
		
			for(int i=1;i<5;i++)//对面守门员太恶心，先让一下,哎，你们谁是守门员啊！
			{
				if(Distance(g->opp[oppid].pos,g->robot[i].pos)<=2*(RR+BR)+1)
				{
						pos.z=0;
				pos.x = FRIGHT-(LRIGHT-FLEFT);
				pos.y =FBOT;//退下！

					Through(g,i,pos);

				}
			}

		}
	    

}

//在边路或者底线推球
int PushBall(Game *g)
{
	double LengthDistance, WidDistance;//球到边路或者底线的距离
	double RunState[5] = {-1, -1, -1, -1, -1};//如果球员可以启动冲，应该冲的方向
	int SituationState = 0;//标记球是否在边路或者底线
	Vector3D Destination[5];//存储队员的应该跑向的位置
	int TurnBackAngle[6] = {-1, -90, 0, 90, 180};//挡住球的去路时，不同情况的绕行方向
	int FlagPrevent = 0;//用位运算实现hash
	int NearestPlayer = -1;//距离球最近的球员
	int hash[5] = {0};//当推球时，标记队员是否有阻挡住球的去路的趋势，不同的数值表示不同的方向
	bool Forbid = false;//标记守门员是否为准备前往参与推球的队员让路过

	//计算到边界的距离
	LengthDistance = g->ball.pos.y - FBOT < FTOP - g->ball.pos.y ? g->ball.pos.y - FBOT : FTOP - g->ball.pos.y;//离上下边距离
	WidDistance = g->ball.pos.x - FLEFT < FRIGHT - g->ball.pos.x ? g->ball.pos.x - FLEFT : FRIGHT - g->ball.pos.x;//离左右边距离


	//处理边路或者底线攻防相关信息
	if (LengthDistance < 2 * RR)//边路攻防   //2*RR
	{
		int i;
		double yray;
		if (LengthDistance == FTOP - g->ball.pos.y)//左边路
		{
			yray = FTOP - RR - 1;
		}
		else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
		{
			yray = FBOT + RR + 1;
		}
		for (i = 1; i < 5; i++)//计算队员的跑位目标
		{
			Destination[i].x = g->ball.pos.x - RR * (3 * i - 2);//  
			Destination[i].y = yray;
			Destination[i].z = 0;
			if (Destination[i].x < FLEFT + 6)
			{
				break;
			}
		}
		int id = 0;
		for (; i < 5; i++)//计算队员的跑位目标
		{
			if (LengthDistance == FTOP - g->ball.pos.y)//左边路
			{
				Destination[i].x = FLEFT + RR + 1;
				Destination[i].y = FTOP - 6 - id * 3 * RR;
				Destination[i].z = 90;
			}
			else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
			{
				Destination[i].x = FLEFT + RR + 1;
				Destination[i].y = FBOT + 6 + id * 3 * RR;
				Destination[i].z = -90;
			}
			id++;
		}
		for (i = 1; i < 5; i++)//如果球员可以启动冲，计算应该冲的方向
		{
			if (LengthDistance == FTOP - g->ball.pos.y)//左边路
			{
				if (FTOP - g->robot[i].pos.y < 3 * RR)
				{
					double angle = atan((FTOP - g->robot[i].pos.y - RR) / (2 * RR)); 
					RunState[i] = angle;
				}
				else if (g->robot[i].pos.x - FLEFT < 3 * RR)
				{
					double angle = atan((g->robot[i].pos.x - FLEFT - RR) / (2 * RR)); 			
					RunState[i] = 90 + angle;
				}
			}
			else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
			{
				if (g->robot[i].pos.y - FBOT < 3 * RR)
				{
					double angle = atan((g->robot[i].pos.y - FBOT - RR) / (2 * RR)); 				
					RunState[i] = -angle;
				}
				else if (g->robot[i].pos.x - FLEFT < 3 * RR)
				{
					double angle = atan((g->robot[i].pos.x - FLEFT - RR) / (2 * RR));				
					RunState[i] = -90 - angle;
				}
			}
		}
		SituationState = 1;
	}
	else if (WidDistance < 2 * RR)//底线攻防   //2*RR
	{
		double xray;
		if (WidDistance == g->ball.pos.x - FLEFT)//底线防守
		{
			xray = FLEFT + RR + 1;
			if (g->ball.pos.y > GTOP)//TOP区域防守
			{
				for (int i = 0; i < 5; i++)
				{
					Destination[i].x = xray;
					Destination[i].y = g->ball.pos.y - RR * (3 * i - 2);
					Destination[i].z = 90;
				}
				for (int i = 0; i < 5; i++)//如果球员可以启动冲，计算应该冲的方向
				{
					if (g->robot[i].pos.x - FLEFT < 3 * RR)
					{
						double angle = atan((g->robot[i].pos.x - FLEFT - RR) / (2 * RR));            //不明白转这个角度				
						RunState[i] = 90 + angle;
					}
				}
				SituationState = 2;
			}
			else if (g->ball.pos.y < GBOT)//BOT区域防守
			{
				for (int i = 0; i < 5; i++)                                                         //这里可以改 防止有2个人在小禁区内
				{
					Destination[i].x = xray;
					Destination[i].y = g->ball.pos.y + RR * (3 * i - 2);
					Destination[i].z = -90;
				}
				for (int i = 0; i < 5; i++)//如果球员可以启动冲，计算应该冲的方向
				{
					if (g->robot[i].pos.x - FLEFT < 3 * RR)
					{
						double angle = atan((g->robot[i].pos.x - FLEFT - RR) / (2 * RR));              //不明白为什么转这个角度				
						RunState[i] = -90 - angle;
					}
				}
				SituationState = 3;
			}
		}
		else if (WidDistance == FRIGHT - g->ball.pos.x)//底线进攻
		{
			xray = FRIGHT - RR - 1;
			if (g->ball.pos.y > (GTOP + GBOT) / 2)//TOP区域进攻
			{
				int i;
				for (i = 1; i < 5; i++)
				{

					Destination[i].x = xray;
					Destination[i].y = g->ball.pos.y + RR * (3 * i - 2);
					Destination[i].z = -90;
					if (Destination[i].y > FTOP)
					{
						break;
					}
				}
				int id = 0;
				for (; i < 5; i++)
				{

					Destination[i].y = FTOP - RR - 1;
					Destination[i].x = FRIGHT - 6 - id * 3 * RR;
					Destination[i].z = 0;
					id++;
				}
				for (i = 1; i < 5; i++)//如果球员可以启动冲，计算应该冲的方向
				{
					if (FRIGHT - g->robot[i].pos.x < 3 * RR)
					{
						double angle = atan((FRIGHT - g->robot[i].pos.x - RR) / (2 * RR));      
						RunState[i] = -90 + angle;
					}
					else if (FTOP - g->robot[i].pos.y < 3 * RR)
					{
						double angle = atan((FTOP - g->robot[i].pos.y - RR) / (2 * RR));              ///同上				
						RunState[i] = angle;
					}
				}
				SituationState = 4;
			}
			else if (g->ball.pos.y < (GTOP + GBOT) / 2)//BOT区域进攻
			{
				int i;
				//push_advanced(g);
				for (i = 1; i < 5; i++)
				{
					Destination[i].x = xray;
					Destination[i].y = g->ball.pos.y - RR * (3 * i - 2);
					Destination[i].z = 90;
					if (Destination[i].y < FBOT)
					{
						break;
					}
				}
				int id = 0;
				for (; i < 5; i++)
				{
					Destination[i].y = FBOT + RR + 1;
					Destination[i].x = FRIGHT - 6 - id * 3 * RR;
					Destination[i].z = 0;
					id++;
				}
				for (i = 1; i < 5; i++)//如果球员可以启动冲，计算应该冲的方向
				{
					if (FRIGHT - g->robot[i].pos.x < 3 * RR)
					{
						double angle = atan((FRIGHT - g->robot[i].pos.x - RR) / (2 * RR));        ///依然决然不解
						RunState[i] = 90 - angle;
					}
					else if (g->robot[i].pos.y - FBOT < 3 * RR)
					{
						double angle = atan((g->robot[i].pos.y - FBOT - RR) / (2 * RR));        ///同上
						RunState[i] = -angle;
					}
				}
				SituationState = 5;
			}
		}
	}

	if (SituationState == 0)
	{
		return SituationState;
	}

	if (SituationState == 2 || SituationState == 3)
	{
		AvoidFouls1(g, SituationState);
	}

	int StartPlayer = 1;
	if (SituationState == 2 || SituationState == 3)            ///为何这里要分开写？
	{
		StartPlayer = 0;
	}

	//当推球时，标记队员是否有阻挡住球的去路的趋势
	//若有，规定球员前进的方向，不同的数值表示不同的方向
	//1、下，2、右，3、上，4、左
	for (int i = StartPlayer; i < 5; i++)
	{
		if (SituationState == 1)
		{
			if (g->robot[i].pos.x >= g->ball.pos.x)
			{
				if (LengthDistance == FTOP - g->ball.pos.y)//左边路
				{
					if (FTOP - g->robot[i].pos.y <= 4 * RR)
					{
						hash[i] = 1; 
					}
					else 
						if (FTOP - g->robot[i].pos.y <= 6 * RR)
						{
							hash[i] = 4;
						}
				}
				else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
				{
					if (g->robot[i].pos.y - FBOT <= 4 * RR)
					{
						hash[i] = 3;
					}
					else 
						if (g->robot[i].pos.y - FBOT <= 6 * RR)
						{
							hash[i] = 4;
						}
				}
			}
		}
		if (SituationState == 2)
		{
			if (g->robot[i].pos.y >= g->ball.pos.y)
			{
				if (g->robot[i].pos.x - FLEFT <= 4 * RR)
				{
					hash[i] = 2;
				}
				else 
					if (g->robot[i].pos.x - FLEFT <= 6 * RR)
					{
						hash[i] = 1;
					}
			}
		}
		if (SituationState == 3)
		{
			if (g->robot[i].pos.y <= g->ball.pos.y)
			{
				if (g->robot[i].pos.x - FLEFT <= 4 * RR)
				{
					hash[i] = 2;
				}
				else 
					if (g->robot[i].pos.x - FLEFT <= 6 * RR)
					{
						hash[i] = 3;
					}
			}
		}
		if (SituationState == 4)
		{
			if (g->robot[i].pos.y <= g->ball.pos.y)
			{
				if (FRIGHT - g->robot[i].pos.x <= 4 * RR)
				{
					hash[i] = 4;
				}
				else 
					if (FRIGHT - g->robot[i].pos.x <= 6 * RR)
					{
						hash[i] = 3;
					}
			}	
		}
		if (SituationState == 5)
		{
			if (g->robot[i].pos.y >= g->ball.pos.y)
			{
				if (FRIGHT - g->robot[i].pos.x <= 4 * RR)
				{
					hash[i] = 4;
				}
				else 
					if (FRIGHT - g->robot[i].pos.x <= 6 * RR)
					{
						hash[i] = 1;
					}
			}
		}
	}


	//统计已经到位参与挤球的球员
	int PlaceId = StartPlayer;// SituationState=2或3时，startplayer=0，否则为1
	for (int i = StartPlayer; i < 5; i++)
	{
		if (RunState[i] != -1 && hash[i] == 0)
		{
			PlaceId++;
		}
	}
	//if(SituationState == 5||SituationState == 6)
	push_advanced(g,SituationState,PlaceId);//处理一些高级动作



	//防止球员多余的相撞
	//在某些情况规定球员前进的方向	
	//1、下，2、右，3、上，4、左
	//处理竖向移动
	Vector3D Pos = Destination[PlaceId];
	int flag = state(Pos);
	int OptState = 0;//状态压缩标记，标记已经被规定移动方向的球员
	for (int i = StartPlayer; i < 5; i++)
	{
		//尚未就位且不阻挡球
		if (RunState[i] == -1 && (hash[i] == 0 || state(Pos) != state(g->ball.pos) || IsInCorner(Pos) != IsInCorner(g->ball.pos)))
		{
			if (flag == 1)
			{
				if (g->robot[i].pos.x > Pos.x)
				{
					if (Pos.y > (FTOP + FBOT) / 2)
					{
						if (FTOP - g->robot[i].pos.y <= 4 * RR)
						{
							hash[i] = 1; 
							OptState |= (1 << i);
						}
					}
					else 
					{
						if (g->robot[i].pos.y - FBOT <= 4 * RR)
						{
							hash[i] = 3;
							OptState |= (1 << i);
						}
					}
				}
			}
			else if (flag == 2)
			{
				if (g->robot[i].pos.y >= Pos.y)
				{
					if (g->robot[i].pos.x - FLEFT <= 4 * RR)
					{
						hash[i] = 2;
						OptState |= (1 << i);
					}
				}	
			}
			else if (flag == 3)
			{
				if (g->robot[i].pos.y <= Pos.y)
				{
					if (g->robot[i].pos.x - FLEFT <= 4 * RR)
					{
						hash[i] = 2;
						OptState |= (1 << i);
					}
				}
			}
			else if (flag == 4)
			{
				if (g->robot[i].pos.y <= Pos.y)
				{
					if (FRIGHT - g->robot[i].pos.x <= 4 * RR)
					{
						hash[i] = 4;
						OptState |= (1 << i);
					}
				}	
			}
			else if (flag == 5)
			{
				if (g->robot[i].pos.y >= Pos.y)
				{
					if (FRIGHT - g->robot[i].pos.x <= 4 * RR)
					{
						hash[i] = 4;
						OptState |= (1 << i);
					}
				}
			}
		}
	}
	//处理横向移动
	for (int i = StartPlayer; i < 5; i++)
	{
		//如果该球已经被规定移动方向，那么略过
		if (OptState & (1 << i))
		{
			continue;
		}
		//尚未就位且不阻挡球
		if (RunState[i] == -1 && (hash[i] == 0 || state(Pos) != state(g->ball.pos) || IsInCorner(Pos) != IsInCorner(g->ball.pos)))
		{
			if (flag == 1)
			{
				if (g->robot[i].pos.x > Pos.x)
				{
					if (Pos.y > (FTOP + FBOT) / 2)
					{
						if (FTOP - g->robot[i].pos.y > 4 * RR && FTOP - g->robot[i].pos.y <= 6 * RR)
						{
							hash[i] = -4;
							OptState |= (1 << i);
						}
					}
					else 
					{
						if (g->robot[i].pos.y - FBOT > 4 * RR && g->robot[i].pos.y - FBOT <= 6 * RR)
						{
							hash[i] = -4;
							OptState |= (1 << i);
						}
					}
				}
			}
			else if (flag == 2)
			{
				if (g->robot[i].pos.y >= Pos.y)
				{
					if (g->robot[i].pos.x - FLEFT > 4 * RR && g->robot[i].pos.x - FLEFT <= 6 * RR)
					{
						hash[i] = -1;
						OptState |= (1 << i);
					}
				}	
			}
			else if (flag == 3)
			{
				if (g->robot[i].pos.y <= Pos.y)
				{
					if (g->robot[i].pos.x - FLEFT > 4 * RR && g->robot[i].pos.x - FLEFT <= 6 * RR)
					{
						hash[i] = -3;
						OptState |= (1 << i);
					}
				}
			}
			else if (flag == 4)
			{
				if (g->robot[i].pos.y <= Pos.y)
				{
					if (FRIGHT - g->robot[i].pos.x > 4 * RR && FRIGHT - g->robot[i].pos.x <= 6 * RR)
					{
						hash[i] = -3;
						OptState |= (1 << i);
					}
				}	
			}
			else if (flag == 5)
			{
				if (g->robot[i].pos.y >= Pos.y)
				{
					if (FRIGHT - g->robot[i].pos.x > 4 * RR && FRIGHT - g->robot[i].pos.x <= 6 * RR)
					{
						hash[i] = -1;
						OptState |= (1 << i);
					}
				}
			}
		}
	}
	//横向移动，保证 2 * RR * sqrt(2)距离内只有一个点在移动
	int front = -1;//前方的点的ID，初始值为-1
	while (true)
	{
		double lmin = INT_MAX;
		int ID = -1;
		for (int i = StartPlayer; i < 5; i++)
		{
			if (hash[i] < 0)
			{
				double len = Distance(Pos, g->robot[i].pos);
				if (len < lmin)
				{
					lmin = len;
					ID = i;
				}
			}
		}
		if (ID != -1 && (front == -1 || (Distance(g->robot[ID].pos, g->robot[front].pos) < 2 * RR * 1.4142135)))
		{
			hash[ID] = hash[ID] * -1;
		}
		else
		{
			break;
		}
		front = ID;
	}

	//防止球员撞墙，绕不出来
	//GoBack(g);

	//防止球员进入球门（当守门员参与推球时，包含守门员）
	for (int i = StartPlayer; i < 5; i++)
	{
		AvoidInDoor(g, i);
	}

	//球进入大禁区后，最近的球员去充当守门员
	if (IsInLRestricted(g->ball.pos))
	{
		int newKeeper = ChooseDoorKeeper(g);
		Vector3D p;
		if (IsInSRestricted(g->robot[newKeeper].pos))
		{
			if (IsInSRestricted(g->ball.pos))
			{
				Goalie(g, newKeeper);
			}
			else
			{
				p.x = FLEFT + RR + 1;
				if (SituationState == 2)
				{
					p.y = STOP - 2 * RR;
					p.z = 120;
				}
				else if (SituationState == 3)
				{
					p.y = SBOT + 2 * RR;
					p.z = -120;
				}
				Position(g, newKeeper, p, p.z, 1);
			}
		}
		else
		{
			p.x = FLEFT + RR + 1;
			p.y = (FTOP + FBOT) / 2;
			if (SituationState == 2)
			{
				p.z = 90;
			}
			else if (SituationState == 3)
			{
				p.z = -90;
			}
			Position(g, newKeeper, p, p.z, 1);
		}
	}

	//处理转弯
	for (int i = 1; i < 5; i++)
	{
		if (hash[i] != 0)//如果已经有规定动作，那么，不转弯
		{
			continue;
		}
		ThroughCorner(g, i);
	}

	////球被撞开进入一定区域后，布置最后的防守
	//if (IsInLRestricted(g->ball.pos))
	//{
	//	LastDefense(g);
	//}

	//给每个队员分配任务
	for (int i = StartPlayer; i < 5; i++)
	{
		int id;
		if (i == 0)
		{
			id = 0;
		}
		else
		{
			double lmin = INT_MAX;
			for (int j = 1; j < 5; j++)
			{
				double lens = Distance(Destination[i], g->robot[j].pos);
				if ((FlagPrevent & (1 << j)) == 0 && lens < lmin)
				{
					id = j;
					lmin = lens;
				}
			}
		}
		FlagPrevent |= (1 << id);
		if (NearestPlayer == -1 && RunState[id] != -1)
		{
			NearestPlayer = id;
		}
		if (hash[id] < 0)//等待转移到后面，但是现在还不能开始转移，避开拥挤
		{
			continue;
		}
		else if (hash[id] > 0)//规定前进方向
		{	
			if(!AvoidFouls(g, id))   //xmm
				Run_Angle(g, id, TurnBackAngle[hash[id]], 125);
		}
		else if (RunState[id] != -1)//可以冲向球
		{
			if(!AvoidFouls(g,id))   //xmm
				Run_Angle(g, id, RunState[id], 125);

			//处理本方守门员让路问题
			if (SituationState == 1)
			{
				Vector3D NextPos;
				NextPos.x = SRIGHT + 2 * RR;
				NextPos.y = g->robot[0].pos.y;
				NextPos.z = 0;
				if (LengthDistance == FTOP - g->ball.pos.y)//左边路
				{
					if (g->robot[0].pos.x - FLEFT < 3 * RR && g->robot[id].pos.x - FLEFT < 3 * RR && g->robot[0].pos.y > g->robot[id].pos.y)
					{
						Forbid = true;
					}
				}
				else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
				{
					if (g->robot[0].pos.x - FLEFT < 3 * RR && g->robot[id].pos.x - FLEFT < 3 * RR && g->robot[0].pos.y < g->robot[id].pos.y)
					{
						Forbid = true;
					}
				}
				if (Forbid == true)
				{
					Position(g, 0, NextPos, NextPos.z, 1);
				}
			}
		}
		else//尚未就位
		{
			Position(g, id, Destination[PlaceId], Destination[PlaceId].z, 1);

			//处理本方守门员让路问题
			if (SituationState == 1)
			{
				if (Distance(Destination[PlaceId], g->robot[id].pos) < 2 * RR)
				{
					Vector3D NextPos;
					NextPos.x = g->robot[0].pos.x;
					NextPos.z = 0;
					if (LengthDistance == FTOP - g->ball.pos.y)//左边路
					{
						NextPos.y = SBOT;
					}
					else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
					{
						NextPos.y = STOP;
					}
					Position(g, 0, NextPos, NextPos.z, 1);
					Forbid = true;
				}
				else
				{
					Vector3D NextPos;
					NextPos.x = SRIGHT + 2 * RR;
					NextPos.y = g->robot[0].pos.y;
					NextPos.z = 0;
					if (LengthDistance == FTOP - g->ball.pos.y)//左边路
					{
						if (g->robot[0].pos.x - FLEFT < 3 * RR && g->robot[id].pos.x - FLEFT < 3 * RR && g->robot[0].pos.y > g->robot[id].pos.y)
						{
							Forbid = true;
						}
					}
					else if (LengthDistance == g->ball.pos.y - FBOT)//右边路
					{
						if (g->robot[0].pos.x - FLEFT < 3 * RR && g->robot[id].pos.x - FLEFT < 3 * RR && g->robot[0].pos.y < g->robot[id].pos.y)
						{
							Forbid = true;
						}
					}
					if (Forbid == true)
					{
						Position(g, 0, NextPos, NextPos.z, 1);
					}
				}
			}
			PlaceId++;
		}
	}

	//如果球在门线处		
	//if(g->how > 0 && g->ball.gate_angle>120)//狮子甩尾
	if(g->ball.pos.y >= GBOT && g->ball.pos.y <= GTOP && g->ball.pos.x >= (FRIGHT - 2 * RR))//狮子甩尾
	{	
		if(g->robot[NearestPlayer].ToBall.dis<3.8)
		{
			if(g->robot[NearestPlayer].ToBall.z > 60)
				Velocity2(g,NearestPlayer,125,-125);
			else
				if(g->robot[NearestPlayer].ToBall.z < -60)
					Velocity2(g,NearestPlayer,-125,125);
		}
		else
			Kick(g,NearestPlayer,g->robot[NearestPlayer].BestBall,g->BestGate);
	}

	//当球在边路时，守门员是否去帮忙顶球
	bool help = false;
	if (SituationState == 1)
	{
		double angle2 = -1;
		for (int i = StartPlayer; i < 5; i++)
		{
			//如果至少有一个球员冲的规定方向为垂直方向，则有球员延伸到底线，守门员去帮忙
			if (state(g->robot[i].pos) == 2 && RunState[i] != -1)
			{
				angle2 = 90;
				break;
			}
			else if (state(g->robot[i].pos) == 3 && RunState[i] != -1)
			{
				angle2 = -90;
				break;
			}
		}
		if (angle2 != -1)
		{
			AvoidInDoor(g, 0);
			help = true;
			Pos.x = FLEFT + RR + 1;
			Pos.y = (FTOP + FBOT) / 2;
			Pos.z = angle2;
			if (state(g->robot[0].pos) != 2 && state(g->robot[0].pos) != 3)
			{
				if ((g->robot[0].pos.y - Pos.y > 2 * RR && angle2 == 90) || (Pos.y - g->robot[0].pos.y > 2 * RR && angle2 == -90))
				{
					if (g->robot[0].pos.x - FLEFT < 4 * RR)
					{
						Run_Angle(g, 0, 0, 125);
					}
					else if (g->robot[0].pos.x - FLEFT < 6 * RR)
					{
						Run_Angle(g, 0, -1 * angle2, 125);
					}
					else
					{
						Position(g, 0, Pos, Pos.z, 125);
					}
				}
				else
				{
					Position(g, 0, Pos, Pos.z, 125);
				}
			}
			else
			{
				Run_Angle(g, 0, angle2, 125);
			}
		}
	}

	////当球在边路时，守门员是否去帮忙顶球
	//bool help = false;
	//if (SituationState == 1)
	//{
	//	double angle2 = -1;
	//	for (int i = StartPlayer; i < 5; i++)
	//	{
	//		//如果至少有一个球员冲的规定方向为垂直方向，则有球员延伸到底线，守门员去帮忙
	//		if (state(g->robot[i].pos) == 2 && RunState[i] != -1)
	//		{
	//			angle2 = 90;
	//			break;
	//		}
	//		else if (state(g->robot[i].pos) == 3 && RunState[i] != -1)
	//		{
	//			angle2 = -90;
	//			break;
	//		}
	//	}
	//	if (angle2 != -1)
	//	{
	//		AvoidInDoor(g, 0);
	//		help = true;
	//		Pos.x = FLEFT + RR + 1;
	//		if (angle2 == 90)
	//		{
	//			Pos.y = GTOP + RR;
	//		}
	//		else if (angle2 == -90)
	//		{
	//			Pos.y = GBOT - RR;
	//		}
	//		Pos.z = angle2;
	//		bool runstate = (state(g->robot[0].pos) == 2 || state(g->robot[0].pos) == 3);
	//		if (runstate == true)
	//		{
	//			Run_Angle(g, 0, angle2, 125);
	//		}
	//		else
	//		{
	//			if ((g->robot[0].pos.y - Pos.y > 0 && angle2 == 90) || (Pos.y - g->robot[0].pos.y > 0 && angle2 == -90))
	//			{
	//				if (g->robot[0].pos.x - FLEFT < 4 * RR)
	//				{
	//					Run_Angle(g, 0, 0, 125);
	//				}
	//				else if (g->robot[0].pos.x - FLEFT < 6 * RR)
	//				{
	//					Run_Angle(g, 0, -1 * angle2, 125);
	//				}
	//				else
	//				{
	//					Position(g, 0, Pos, Pos.z, 125);
	//				}
	//			}
	//			else 
	//			{
	//				Position(g, 0, Pos, Pos.z, 125);
	//			}
	//		}
	//	}
	//}

	//如果守门员未参与顶球且未为顶球球员让路过
	if (StartPlayer && Forbid == false && help == false)
	{
		Goalie(g,g->r[0]);
	}
	return SituationState;
}


//点球策略1 利用推箱子策略 一个传中 其他去推。 
void dianqiu(Game *g)
{
	Vector3D target;
	target.x = FRIGHT - 6;
	target.y = FBOT - 6;
	target.z = 0;

	if((fabs(g->ball.pos.x - FRIGHT) <= 6 && fabs(g->ball.pos.y - FBOT) <= 6)|| fabs(g->ball.pos.x - FRIGHT) <= 6)
	{
		PushBall(g);
	}
	else
	{
		Kick(g,4,target);
		for(int i = 1; i < 4; i ++)
		{
			Position(g,i,target,0);
		}
	}
}
//点球策略2 自传 + 自T + 晃守门员！！！！！！
void dianqiu2(Game *g)
{
	if(fabs(g->ball.pos.x - 79.5506 ) < 3 && fabs(g->ball.pos.y - 43.0032) < 3 && g->penalty_count < 4)
	{
		Vector3D target;
		target.x = 80;
		//target.y = 56;
		target.y = 29;
		Kick2(g,4,target,100);
	}
	else 
	{
		Vector3D target;
		target.x = 95;
		//target.y = 60;
		target.y = 25;
		Kick(g,4,target);
	}
	//for(int i = 1; i < 4; i ++)
	//Kick(g,i,g->BestGate);
}
//通过推球来射门
void dianqiu3(Game *g)
{
	Vector3D target;
	target.x = FRIGHT - 6;
	target.y = FBOT - 6;
	target.z = 0;

	if((fabs(g->ball.pos.x - FRIGHT) <= 6 && fabs(g->ball.pos.y - FBOT) <= 6)|| fabs(g->ball.pos.x - FRIGHT) <= 6)
	{
		PushBall(g);
	}
	else
	{
		Through(g,4,g->ball.pos);
		for(int i = 1; i < 4; i ++)
		{
			Position(g,i,target,0);
		}
	}
}
void dianqiu4(Game *g)
{
	int i;
	Vector3D target;
	target.x = FRIGHT - 6;
	target.y = FBOT - 6;
	target.z = 0;
	double dis=10000;
	double tmp;
	static int flag;
	int trobot=0;
	/*
	for(i=0;i<5;i++)    //哪个是守门员？
	{
	tmp=Distance(Vect(FRIGHT,(FTOP+FBOT)/2,0),g->opp[i].pos);

	if(dis>tmp)
	{
	dis=tmp;
	trobot=i;
	}
	}
	*/
	Vector3D prerobot;
	prerobot=PredictMan(g,trobot,3);
	if(Distance(prerobot,g->opp[trobot].pos)>RR/2)
	{
		if(flag==1||fabs(prerobot.x-g->robot[trobot].pos.x)<fabs(prerobot.y-g->robot[trobot].pos.y))
		{
			dianqiu(g);
			flag=1;
		}
		else
		{
			dianqiu3(g);
			flag=0;
		}
	}
}

Vector3D intersection(Vector3D u1,Vector3D u2,Vector3D v1,Vector3D v2)
{
	Vector3D ret=u1;
	double t=((u1.x-v1.x)*(v1.y-v2.y)-(u1.y-v1.y)*(v1.x-v2.x))
		/((u1.x-u2.x)*(v1.y-v2.y)-(u1.y-u2.y)*(v1.x-v2.x));
	ret.x+=(u2.x-u1.x)*t;
	ret.y+=(u2.y-u1.y)*t;
	return ret;
}
//点到直线上的最近点
Vector3D ptoline(Vector3D p,Vector3D l1,Vector3D l2){
	Vector3D t=p;
	t.x+=l1.y-l2.y,t.y+=l2.x-l1.x;
	return intersection(p,t,l1,l2);
}
void dianqiu5(Game *g)   //4号是罚球人 但是击球者是离球最近的（除去4号)
{
	int myrobot,opprobot,i;
	static int kk=0;
	Vector3D target;
	target.x = FRIGHT - 6;
	target.y = FBOT - 6;
	target.z = 0;
	double mydis=10000;
	double oppdis=10000;
	double tmp;
	for(i=1;i<4;i++)
	{
		tmp=Distance(g->robot[i].pos,g->ball.pos);
		if(mydis>tmp)
		{
			mydis=tmp;
			myrobot=i;
		}
	}
	for(i=1;i<5;i++)
	{
		tmp=Distance(g->opp[i].pos,g->ball.pos);
		if(oppdis>tmp)
		{
			oppdis=tmp;
			opprobot=i;
		}
	}

	if(g->opp[0].pos.x<90.500)   //90.500
	{
		if(Distance(g->robot[4].pos,g->ball.pos)>BR+RR+0.2)
			Through(g,4,g->ball.pos,125);
		else
			Kick(g,4,target);
	}
	else
	{
		Through(g,4,g->ball.pos,1);
	}

	if(mydis>BR+RR+0.2)
	{

		Through(g,myrobot,g->ball.pos,125);
	}
	else
	{
		Kick(g,myrobot,target);
	}

}
//球进入大禁区后，最近的球员去充当守门员
int ChooseDoorKeeper(Game *g)
{
	int id = -1;
	Vector3D p;
	if (state(g->ball.pos) == 2)
	{
		p.x = FLEFT + RR + 1;
		p.y = STOP - 2 * RR;
		p.z = 120;
	}
	else if (state(g->ball.pos) == 3)
	{
		p.x = FLEFT + RR + 1;
		p.y = SBOT + 2 * RR;
		p.z = -120;
	}
	for (int i = 0; i < 5; i++)
	{
		double lmin = INT_MAX;
		double len = Distance(g->robot[i].pos, p);
		if (id == -1 || len < lmin)
		{
			lmin = len;
			id = i;
		}
	}
	return id;
}


int PreTime(Game *g, int robot,Vector3D pos)
{
	int time=0;//帧数

	for(time=0;time<20;time++)
	{
		
	}
	return 0;
}

void Sweep(Game *g, int which)//截球
{
	int add_time=0;
	Vector3D a;
	add_time= (int)   (  fabs(Distance(g->robot[which].pos,  g->ball.prepos[1]) -3.5 ) / 1.2  ) ;
	add_time=abs(add_time);

	if( Distance(g->robot[which].pos,  g->ball.pos) <3.0  &&   (g->ball.pos.x  -  g->robot[which].pos.x >0.5  )  ) //跑位成功，踢出
	{
		if( g->robot[which].pos.y > g->ball.pos.y ) 
			Velocity(g,which,-125,125);
		else
			Velocity(g,which,-125,125);
	}


	if(add_time>18) //跑到拦截的位置
	{
			a.x=g->ball.prepos[20].x-1.5;
			a.y=g->ball.prepos[20].y;
	}
	else if( add_time==0 )
	{
			a.x=g->ball.prepos[0].x-1.5;
			a.y=g->ball.prepos[0].y;
	}
	else
	{
			a.x=g->ball.prepos[add_time].x-1.5;
			a.y=g->ball.prepos[add_time].y;
	}
	//Position( g,which, a);
	Through(g,which,a);
}
