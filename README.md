/*# Database-of-employee.cpp-
A project on database of employee.
~By Minhajul Mobin*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct emp		//structure of Employee data type

{
    int id;
    char name[20];
    int salary;
};
void append();				//declare many function
void display();
void display_all();
void modify();
void del();
void tsearch();
void rname();
void rremove();
char mygetch();
char fname[]= {"mydb.dat"};

int main()
{
    int ch;
    while(1)
    {
        system("cls");
        printf("==============================Employee management System==============================\n");
        printf("1. Append\n2. Modify\n3. Delete\n4. Search\n5. Disaplay\n6. Disaplay All\n7. Rename\n");
        printf("8. Delete File\n0. Exit\n\n");
        printf("====================================================================================");
        printf("\nPlease enter your Chioce: ");
        scanf("%d",&ch);
        switch(ch)
        {
        case 1:
            append();
            break;
        case 2:
            modify();
            break;
        case 3:
            del();
            break;
        case 4:
            tsearch();
            break;
        case 5:
            display();
            break;
        case 6:
            display_all();
            break;
        case 7:
            rname();
            break;
        case 8:
            rremove();
            break;
        case 0:
            exit(0);
        }
        mygetch();
    }
    return 0;
}

void append()				//for input employees all ditails such as id,name ,salary
{
    FILE *fp;
    struct emp t1;
    fp=fopen(fname,"ab");
    printf("\nEnter ID: ");
    scanf("%d",&t1.id);
    printf("\nEnter Name: ");
    scanf("%s",t1.name);
    printf("Enter salary: ");
    scanf("%d",&t1.salary);
    fwrite(&t1,sizeof(t1),1,fp);
    fclose(fp);
}

void rname()                    //for create new file name
{
    char name[20];
    printf("\nEnter the New File name: ");
    fflush(stdin);
    scanf("%[^\n]",name);
    rename(fname,name);
    strcpy(fname,name);
}

void rremove()                  //for for delete old file name or file
{
    FILE *fp,*fp1;
    struct emp t;
    char name[20];
    char val[20];
    printf("\nDo you want to make copy o it(Y/N): ");
    scanf("%s",val);
    if(strcmp(val,"Y")==0)
    {
        printf("\nEnter the New File name: ");
        fflush(stdin);
        scanf("%[^\n]",name);
        fp=fopen(name,"wb");
        fp1=fopen(fname,"rb");
        while(1)
        {
            fread(&t,sizeof(t),1,fp1);
            if(feof(fp1));
            break;
            fwrite(&t,sizeof(t),1,fp);
        }
        fclose(fp);
        fclose(fp1);
        remove(fname);
        strcpy(fname,name);
    }
    else
    {
        remove(fname);
    }
}

void modify()                               //for modify one employee information
{
    FILE *fp,*fp1;
    struct emp t,t1;
    int id,f=0,c=0;
    fp=fopen(fname,"rb");
    fp1=fopen("temp.dat","wb");
    printf("\nEnter the Emp ID you want to modify: ");
    scanf("%d",&id);
    while(1)
    {
        fread(&t,sizeof(t),1,fp);
        if(feof(fp))
            break;
        if(t.id==id)
        {
            f=1;
            printf("\n Enter ID: ");
            scanf("%d",&t.id);
            fflush(stdin);
            printf("\n Enter Name: ");
            scanf("%s",t.name);
            printf("Enter salary: ");
            scanf("%d",&t.salary);
            fwrite(&t,sizeof(t),1,fp1);
        }
        else
        {
            fwrite(&t,sizeof(t),1,fp1);
        }
    }
    fclose(fp);
    fclose(fp1);
    if(f==0)
    {
        printf("Sorry No Record Found\n\n");
    }
    else
    {
        fp=fopen(fname,"wb");
        fp1=fopen("temp.dat","rb");
        while(1)
        {
            fread(&t,sizeof(t),1,fp1);
            if(feof(fp1)) break;
            fwrite(&t,sizeof(t),1,fp);
        }
        fclose(fp);
        fclose(fp1);
    }
}

void del()                                  //for delete one employee information
{
    FILE *fp,*fp1;
    struct emp t,t1;
    int id,f=0,c=0;
    fp=fopen(fname,"rb");
    fp1=fopen("temp.dat","wb");
    printf("\nEnter the Emp ID you want to Delete: ");
    scanf("%d",&id);
    while(1)
    {
        fread(&t,sizeof(t),1,fp1);
        if(feof(fp)) break;
        if(t.id==id) f=1;
        else fwrite(&t,sizeof(t),1,fp1);
    }
    fclose(fp);
    fclose(fp1);
    if(f==0)
    {
        printf("Sorry No Record Found\n\n");
    }
    else
    {
        fp=fopen(fname,"wb");
        fp1=fopen("temp.dat","rb");
        while(1)
        {
            fread(&t,sizeof(t),1,fp1);
            if(feof(fp)) break;
            fwrite(&t,sizeof(t),1,fp);
        }
        fclose(fp);
        fclose(fp1);
    }
}

void display()                      //for display one  search by id
{
    FILE *fp;
    struct emp t;
    int id,f=0;
    fp=fopen(fname,"rb");
    printf("\nEnter the Emp ID: ");
    scanf("%d",&id);
    while(1)
    {
        fread(&t,sizeof(t),1,fp);
        if(feof(fp)) break;
        if(t.id==id)
        {
            f=1;
            printf("\n====================================================================================\n");
            printf("\t\tEmployee Details of %d\n\n",t.id);
            printf("\n====================================================================================\n");
            printf("Name\tSalary\n\n");
            printf("%s\t",t.name);
            printf("%d\t\n\n",t.salary);
            printf("\n====================================================================================\n");
        }
    }
    if(f==0)
        printf("\nSorry No Record Found");
    fclose(fp);
}

void tsearch()                    //for get only one employee information search by name
{
    FILE *fp;
    struct emp t;
    int f=0;
    char n[20];
    fp=fopen(fname,"rb");
    printf("\nEnter the Employee Name: ");
    scanf("%s",&n);
    while(1)
    {
        fread(&t,sizeof(t),1,fp);
        if(feof(fp)) break;
        if(strcmp(n,t.name)==0)
        {
            f=1;
            printf("\n====================================================================================\n");
            printf("\t\tEmployee Details of %d\n\n",t.id);
            printf("\n====================================================================================\n");
            printf("Name\tSalary\n\n");
            printf("%s\t",t.name);
            printf("%d\t\n\n",t.salary);
            printf("\n====================================================================================\n");
        }
    }
    if(f==0) printf("\nSorry No Record  Found");
    fclose(fp);
}

void display_all()            //display all employes: Id, Name, Salary
{
    FILE *fp;
    struct emp t;
    fp=fopen(fname,"rb");
    printf("\n====================================================================================\n");
    printf("\t\t\tEmployee Details of all:\n\n");
    printf("\n====================================================================================\n");
    printf("Roll No\tName\tSalary\n\n");
    while(1)
    {
        fread(&t,sizeof(t),1,fp);
        if(feof(fp)) break;
        printf("%d\t",t.id);
        printf("%s\t",t.name);
        printf("%d\t\n\n",t.salary);
    }
    printf("\n====================================================================================\n");
    fclose(fp);
}

char mygetch()      //this function work as gets function
{
    char val;
    char rel;
    scanf("%c",&val);
    scanf("%c",&rel);
    return (val);
}
