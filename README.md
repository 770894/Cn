# STA
check=True while check:

a,b,c=map(int,input().split())

c1=((a>=1)) and ((a<=10))

c2=((b>=1)) and ((b<=10))

c3=((c>=1)) and ((c<=10))

if(not c1):

print("a is not in range") if(not c2):

print("b is not in range") if(not c3):

print("c is not in range") check=(not c1) or (not c2) or (not c3)

if((a<(b+c)) and (b<(a+c)) and (c<(a+b))): if((a==b) and (b==c)):

print("Eqilateral triangle") elif((a!=b) and (b!=c) and (c!=a)):

print("Scalene triangle") else:

print("Isosceles triangle")

else:

print("Triangle cannot be formed")

Output:








2)------------
Program:

#include<stdio.h>

int main()

{

int c1,c2,c3,temp;

int locks,stocks,barrels,totallocks,totalstocks,totalbarrels;

float lockprice,stockprice,barrelprice,locksales,stocksales,barrelsales,sales,com; lockprice=45.0; stockprice=30.0;

barrelprice=25.0; totallocks=0; totalstocks=0; totalbarrels=0;

printf("Enterthe number oflocks and to exit press-1\n"); scanf("%d",&locks);

while(locks != -1)

{

c1=(locks<=0 || locks>70);

printf("\nEnterthe number ofstocks and barrels\n"); scanf("%d %d",&stocks,&barrels);

c2=(stocks<=0 || stocks>80); c3=(barrels<=0 || barrels>90); if(c1)

printf("\nValue oflocks are not in the range of 1.	70\n");

else

{

temp=totallocks+locks; if(temp>70) printf("Newtotallocks=%dnotintherangeof1.	70\n",temp);

else totallocks=temp;

}
printf("Total locks = %d",totallocks); if(c2)

printf("\n Value ofstocks not in the range of 1.	80\n");

else

{

temp=totalstocks+stocks;

if(temp>80)

printf("\nNewtotalstocks =%d not in the range of 1	80",temp);

else totalstocks=temp;

}

printf("\nTotal stocks = %d",totalstocks); if(c3)

printf("\n Value of barrels not in the range of 1.	90\n");

else

{

temp=totalbarrels+barrels; if(temp>90)

printf("\nNewtotal barrels=%dnotin the range of 1	90\n",temp);

else totalbarrels=temp;

}

printf("\nTotal barrels=%d", totalbarrels);

printf("\nEnterthe number of locks and to exit press-1\n"); scanf("%d",&locks);

}
printf("\n Total locks = %d",totallocks); printf("\nTotal stocks = %d",totalstocks); printf("\n Total barrels = %d",totalbarrels);

locksales=totallocks*lockprice;

stocksales=totalstocks*stockprice; barrelsales=totalbarrels*barrelprice; sales=locksales+stocksales+barrelsales; printf("\nTotal sales = %f",sales); if(sales>1800)

{ com=0.10*1000;

com=com+(0.15*800); com=com+0.20*(sales-1800);

}

else if(sales>1000)

{ com=0.10*1000;

com=com+0.15*(sales-1000);

}

else com=0.10*sales;

printf("\nCommission = %f",com); return 0;

}

Output:












3)--------+++++++------

EXNO: 3. Design, Develop, code and run the program in any suitable language to       implement the Next Date function. Analyze it from the perspective of Equivalence Class Testing, derive different testcases, execute these testcases and discuss the results.
DATE:


AIM:

Program:

#include<stdio.h>
int check(int day,int month)
{
if((month==4||month==6||month==9 ||month==11) && day==31) return 1;
else
return 0;
}

int isleap(int year)
{
if((year%4==0 && year%100!=0) || year%400==0) return 1; else
return 0;
}
int main()
{
int day,month,year,tomm_day,tomm_month,tomm_year; char flag; do
{
flag='y';
printf("\nenter the today's date in the form of dd mm yyyy\n"); scanf("%d%d%d",&day,&month,&year); tomm_month=month; tomm_year= year; if(day<1 || day>31)
{
printf("value of day, not in the range 1...31\n"); flag='n';
}
if(month<1 || month>12)
{
printf("value of month, not in the range 1.	12\n"); flag='n';
}
else if(check(day,month))
{
printf("value of day, not in the range day<=30"); flag='n';
}

if(year<=1812 || year>2015)
{
printf("value of year, not in the range 1812. 2015\n"); flag='n';
}
if(month==2)
{
if(isleap(year) && day>29)
{
printf("invalid date input for leap year"); flag='n';
}
else if(!(isleap(year))&& day>28)
{
printf("invalid date input for not a leap year"); flag='n';
}
}
}while(flag=='n');
{
case 1:
case 3:
case 5:
case 7:
case 8:

case 10:if(day<31) tomm_day=day+1;

else
{
tomm_day=1; tomm_month=month+1;
}
break; case 4:
case 6:
case 9:
case 11: if(day<30) tomm_day=day+1; else
{
tomm_day=1; tomm_month=month+1;
}
break;
case 12: if(day<31) tomm_day=day+1; else
{
tomm_day=1; tomm_month=1; if(year==2015)
{
printf("the next day is out of boundary value of year\n"); tomm_year=year+1;
}
else tomm_year=year+1;
}
break; case 2:
if(day<28) tomm_day=day+1;
else if(isleap(year)&& day==28) tomm_day=day+1; else if(day==28 || day==29)
{
tomm_day=1; tomm_month=3;
}
break;
}
printf("next day is : %d %d %d",tomm_day,tomm_month,tomm_year); return 0;
}

Output:
