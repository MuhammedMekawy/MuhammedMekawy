#include<iostream>
#include<string>
#include<cmath>
#include<cctype>


using namespace std;


int main()
{
    cout<<"Welcome to the advanced circuit analyzer, please enter your electrical circuit configuration:\n('S' for series,'P' for parallel,end each connection by 'e',Separate characters by spaces or commas)\n\n";
    string s;//string contains circuit configuration
    getline(cin,s);
    int length=s.length();//number of characters input
    int connections=0;//number of series and parallel connections to be reduced
    for(int i=0,e=0;i<length;i++)
    {
        if(s[i]!=32 && s[i]!=44 && s[i]!=46 && (s[i]<48 || s[i]>57) && toupper(s[i])!=80 && toupper(s[i])!=83 && s[i]!=101)
        {
            cout<<"Wrong Description";
            exit(0);
            //checks that the connections are either S or P and characters are separated by spaces or commas only
        }

        if(toupper(s[i])==80 || toupper(s[i])==83)
            connections++;//counts the number of parent connections

        if(s[i]==101)
            e++;//counts the number of times the user ends a connection

        if(i==(length-1))
        {
            if(connections==0)
            {
                cout<<"Please enter at least one type of connection\n";
                exit(0);
                //checks that user input at least one type of connection
            }

            if(connections!=e)
            {
                cout<<"please end each connection by 'e'\n";
                exit(0);
                //checks that each connection is ended by 'e'
            }
        }
    }
    int start=0,endd=0;
    string subs="";//will contain substring of string s to be reduced
    int subs_length=0;//length of each new substring

    for(int i=0;i<connections;i++)//to replace each parent connection by equivalent resistance
    {
        int updated_length=s.length();
        for(int i=0;i<updated_length;i++)
        {
            if(toupper(s[i])==80 || toupper(s[i])==83)
            {
                start=i;//the start index of the circuit connection to be reduced
            }
            if(s[i]==101)
            {
                endd=i;//the end index of the circuit to be reduced
                break;
            }
        }
        subs_length=endd-start+1;//takes the appropriate connection to reduce it in a substring alone
        subs=s.substr(start,subs_length);//substring to be replaced by a equivalent value
        int subs_resistors=-1;//number of resistors in each substring by calculating number of spaces
        for(int i=0;i<subs_length;i++)
        {
            if(subs[i]==' '||subs[i]==',')
                subs_resistors++;//counts the number of resistors in each parent connection
        }

        float resistors[20];//array contains the values of resistors to be replaced by equivalent resistor
        int start_array=0;//to place each value of resistor in successive array index
        int startf=2,endf=0;//the start and end indexes of each value of resistor
        for(int i=3;i<(subs_length-1);i++)
        {
            if(subs[i]==' '||subs[i]==',')
            {
                endf=i;
                string r=subs.substr(startf,(endf-startf));
                resistors[start_array]=stof(r);//place the value of the resistor in the substring into array contains resistances
                start_array++;
                startf=(endf+1);

            }
        }
        float Requ=0;//the equivalent resistance of each connection

        switch(toupper(subs[0]))
        {
        case 'S':
            if(subs_resistors<1)
            {
                cout<<"Incorrect Input";
                exit(0);
                //checks that series connections must have 1 resistor or more
            }
            for(int i=0;i<subs_resistors;i++)
            {
                Requ+=resistors[i];//calculates equivalent resistance in case of series connection
            }
            break;
        case 'P':
            if(subs_resistors<2)
            {
                cout<<"Incorrect Input";
                exit(0);
                //checks that parallel connections must have 2 resistors or more
            }
            for(int i=0;i<subs_resistors;i++)
            {
                if(resistors[i]==0)
                {
                    Requ=9999999999;//to give 0 equivalent resistance when reciprocated
                    break;
                    //in case of short circuit
                }
                Requ+=(1/resistors[i]);//calculates the reciprocal of the equivalent resistance in case of parallel connection
            }
            Requ=(1/Requ);//calculates the equivalent resistance in case of parallel connection
            break;
        default:
            cout<<"Incorrect Input";
            exit(0);
        }
        s.replace(start,(endd-start+1),to_string(Requ));//rewrite the circuit configuration after reduction
    }
    float R=stof(s);//equivalent resistance
    cout<<"The total resistance = "<<R<<endl;

}



<!---
MuhammedMekawy/MuhammedMekawy is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
