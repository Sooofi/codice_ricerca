# codice_ricerca
Consegna esercizio 
#include <iostream>
#include <stdlib.h>
#include <fstream>
#include <string>
#define DIM 3

/*Scrivere un programma in C++ che gestisca la rubrica telefonica.
struct Rubrica(int codiceContatto, n_telefono, string nome, cognome);
scrivere i dati all'interno di un vettore:  struct contatto[200];
1) inserire e scrivere i dati su di un file in modalità binaria.
2) leggere la rubrica dal file e stamparla in output;
2) ricercare il contatto memorizzato sul file;*/

using namespace std;

///struct
struct rubrica{
    int codiceContatto;              //codice ID contatto
    string n_telefono;               //numero di telefono
    string nome;                    //nome
    string cognome;                //cognome
}contatto[DIM];

void inserimento()
{

    ofstream file("MioFile.dat",ios::out|ios::binary);
    if(!file)
        cout<<"errore apertura file ";
    else
    {
        string temp;
        for(int i=0;i<DIM;i++)
        {
            ///carico la struct
            cout<<"Inserimento del contatto n."<<(i+1)<<endl;
            contatto[i].codiceContatto=(i+1);
            cout<<"Numero telefonico: ";
            fflush(stdin);
            getline(cin,temp);
            contatto[i].n_telefono=temp;
            cout<<endl;
            cout<<"nome contatto:";
            fflush(stdin);
            getline(cin,temp);
            contatto[i].nome=temp;
            cout<<endl;
            cout<<"cognome contatto:";
            fflush(stdin);
            getline(cin,temp);
            contatto[i].cognome=temp;
            cout<<endl;
            ///carico la struct sul file tramite il metodo write
            file.write((char *)&contatto[i],sizeof(contatto[i]));
        }
file.close();

    }
}

void stampa()
{
     ifstream file("MioFile.dat",ios::in|ios::binary);
    if(!file)
        cout<<"errore apertura file ";
    else
    {
        for(int i=0;i<DIM;i++)
        ///attraverso il metodo read carico il file sulla struct
            file.read((char *)&contatto[i],sizeof(contatto[i]));
    for(int c=0; c<DIM; c++)
          {
             cout<<contatto[c].codiceContatto<<" "<<"Numero di telefono: "<<contatto[c].n_telefono<<" di "<<contatto[c].cognome<<" "<<contatto[c].nome<<endl;

          }
    }
    file.close();

}

void ricerca()
{
    ifstream file("MioFile.dat",ios::in|ios::binary);
    if(!file)
        cout<<"errore apertura file "<<endl;
    else
    {
        rubrica temp;
        long codic,input=0;
        while(file.read((char *)&temp,sizeof(temp)))
            input++;
        cout<<"Inserire codice da trovare: ";
        cin>>codic;
        if(codic<=input)
        {
            file.seekg(codic*sizeof(temp));
            file.read((char*)&temp,sizeof(temp));
            cout<<temp.codiceContatto<<": "<<temp.n_telefono<<"\t"<<temp.cognome<<" "<<temp.nome<<endl;
        }
        else
            cout<<"ID non presente nel file.";
    }
    file.close();
}

int main()
{
    int scelta;
    do
    {
        do
        {
            cout<<"Quale operazione vuoi eseguire?"<<endl;
            cout<<"1)Inserimento dati"<<endl;
            cout<<"2)Stampa"<<endl;
            cout<<"3)Ricerca del contatto"<<endl;
            cout<<"4)Exit"<<endl;
            cin>>scelta;
        }
        while(scelta<1||scelta>4);
         system("cls");
        switch(scelta)
        {
            case 1:inserimento();
                   break;
            case 2:
                   stampa();
                   break;
            case 3:ricerca();
                   break;
            case 4:cout<<"Fine programma XD";
                   break;
        }
    }
    while(scelta!=4);
    return 0;
}


