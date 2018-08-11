#include<iostream>
#include<GL/glut.h>
#include<math.h>
using namespace std;

double xmin,ymin,xmax,ymax,newxmax,newxmin,newymax,newymin;

void draw_pixel(int x,int y)
{
	glColor3f(1.0,1.0,1.0);
	glBegin(GL_POINTS);
	glVertex2i(x,y);
	glEnd();
}
int sign(int x)
{
    if(x > 0) return 1;
    if(x < 0) return -1;
    return 0;
}

void drawline(double X1,double Y1, double X2,double Y2)
{
	float x,y,dx,dy,length;
	int i;
	
	dx=abs(X2-X1);
	dy=abs(Y2-Y1);
	if(dx>=dy)
		length=dx;
	else
		length=dy;
	dx=(X2-X1)/length;
	dy=(Y2-Y1)/length;
	x=X1+0.5*sign(X1);
	y=Y1+0.5*sign(Y1);
	i=1;
	while(i<=length)
	{
		draw_pixel(int (x),int (y));
		x=x+dx;
		y=y+dy;
		i=i+1;
	}
	glFlush();

}
void display(void)
{
	
	glClear(GL_COLOR_BUFFER_BIT);
	glColor3f(1.0,1.0,0.0);
	glBegin(GL_LINES);
	glEnd();
	//Outer rectangle
	drawline(xmin,ymin,xmax,ymin);
	drawline(xmax,ymin,xmax,ymax);
	drawline(xmax,ymax,xmin,ymax);
	drawline(xmin,ymax,xmin,ymin);	
	//rhombus inside rectangle
	drawline(xmin,(ymin+ymax)/2,(xmin+xmax)/2,ymin);
	drawline((xmin+xmax)/2,ymin,xmax,(ymin+ymax)/2);
	drawline(xmax,(ymin+ymax)/2,(xmin+xmax)/2,ymax);
	drawline((xmin+xmax)/2,ymax,xmin,(ymin+ymax)/2);

	//inner rectangle inside rhombus

	newxmin=xmin+(xmax-xmin)/4;
	newxmax=xmax-(xmax-xmin)/4;
	newymin=ymin+(ymax-ymin)/4;
	newymax=ymax-(ymax-ymin)/4;

	drawline(newxmin,newymin,newxmax,newymin);
	drawline(newxmax,newymin,newxmax,newymax);
	drawline(newxmax,newymax,newxmin,newymax);
	drawline(newxmin,newymax,newxmin,newymin);	


}
void init(void)
{
	glClearColor(0.0,0.0,0.0,0.0);
	gluOrtho2D(0.0,500.0,0.0,500.0);
}
int main(int argc,char** argv)
{
	

	cout<<"\nEnter left bottom coordinates of rectangle : ";
	cin>>xmin>>ymin;

	cout<<"\nEnter right top coordinates of rectangle : ";
	cin>>xmax>>ymax;
	

	glutInit(&argc,argv);
	glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
	glutInitWindowSize(500,500);
	glutInitWindowPosition(0,0);
	glutCreateWindow("DDA Figure");
	init();
	glClear(GL_COLOR_BUFFER_BIT);
	glutDisplayFunc(display);
	glFlush();
	glutMainLoop();

	return 0;
	
}
