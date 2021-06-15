// STAJ-YONETIM-SISTEMI C KODU
#include <stdio.h>
#include <stdlib.h>
   int ogrsayisi=0;
   int firmasayisi=0;
   int stajsayisi=0;

struct ogrenci
{
	char ad[30];
	char soyad[20];
	char bolum[100];
	int sinif;
	int no;
	int id;
	int stajdurumu;
}ogr[100];

struct firma
{
	char ad[30];
	char calismalani[50];
	int vergino;
	int id;
}fir[100];

struct staj
{
	char baslangic[20];
	char bitis[20];
	int  haftasayisi;
	char stajturu[50];
	int ogrid;                  // ogrenci ve firma bilgilerinde duzenlendiginde staj bilgilerinin eslenmesi id ile saglanir
	int firid;                  //
}stj[100];
    int ogrencilistele();
	int ogrencianafonk();
	int ogrencibilgial();
	void fileogrenciekle (char ad[30], char soyad[20],int no,char bolum[100],int sinif,int id);
	void fileogrencioku ();
	int ogrencibul(int duzensil);
	void ogrenciduzenle(int f);
	void ogrencisil(int f);
	void fileogrenciduzenle ();
    int ogrencilistele();
    int firmaanafonk();
    int firmaekle();
    void filefirmaekle (char ad[30], char alani[50],int vno,int id);
    void filefirmaoku ();
    int firmabul(int duzensil);
    void firmaduzenle(int f);
	void firmasil(int f);
    void filefirmaduzenle ();
	int firmalistele();
	int stajanafonk();
	int stajekle();
	void filestajekle (char bastarih[20], char bitistarih[20],int stajhafta,char stajturu[50], int ogrno, int firmano );
    void yarimstaj();
    void bitmisstaj();
    int stajdurumu (int durum);
    void filestajoku();

int main()
{
	char ch;
	struct ogrenci *optr=&ogr[0];
	struct firma *fptr=&fir[0];
	struct staj *sptr=&stj[0];
    do
	{
		fileogrencioku ();
		filefirmaoku ();
		filestajoku();
		printf("\tMENU");
		printf("\n1. Ogrenci islemleri");
		printf("\n2. Firma islemleri");
		printf("\n3. Staj islemleri\n");
		scanf("%c",&ch);
        system("cls");
    		switch(ch)
		{
		case '1':
			ogrencianafonk();
			break;
	 	case '2':
			firmaanafonk();
			break;
		case '3':
			stajanafonk();
			break;
		}
	}while(ch!='8');
return 0;
}
//***********************************************ogrenci*****************************************************
	int ogrencianafonk(){

		char ch;
		int sonlandir;
		do
	{
		sonlandir=1;
		printf("\n\n  Ogrenci MENU");
		printf("\n\t1. Ogrenci ekleme");
		printf("\n\t2. Ogrenci duzenleme");
		printf("\n\t3. Ogrenci silme");
		printf("\n\t4. Ogrencileri listeleme");
		printf("\n\tSeciminizi Belirtiniz (1-4) \n");
		scanf("%s",&ch);
		switch(ch)
		{
		case '1':
			sonlandir=ogrencibilgial();
			break;

		case '2':
		    sonlandir=ogrencibul(1);
            break;

		case '3':
		    sonlandir=ogrencibul(2);
            break;

        case '4':
		    sonlandir=ogrencilistele();
            break;
		}
	}while((ch!='0')&&(sonlandir!=0));
    return 1;
	}
//**********************************ogrenci ekleme****************************************************************
    int ogrencibilgial(){
    int i;
    struct ogrenci *optr=&ogr[0];
    int k,no,sinif,id,don;
    char ad[30],soyad[20],bolum[100];
	printf("Ogrencinin adini giriniz: \n");
	scanf("%s",ad);
	printf("%s isimli ogrencinin soyadini giriniz: \n",ad);
	scanf("%s",soyad);
	printf("%s isimli ogrencinin numarasini giriniz: \n",ad);
	scanf("%d" ,&no);
	for(k=ogrsayisi-1;k>=0;k--){
		if(optr[k].no == no){
			printf("%s isimli ogrenci zaten sisteme kayitlidir.\n",&ad);
			return 1;
		}
	}
	printf("%s isimli ogrencinin bolumunu giriniz: \n",ad);
	scanf("%s",bolum);
	printf("%s isimli ogrencinin sinifini giriniz: \n",ad);
	scanf("%d",&sinif);
	id=ogrsayisi+1;
	fileogrenciekle (ad, soyad, no, bolum, sinif, id); //ogrenci.txt olusturma ve bilgileri ekleme fonkiyonu
	printf("Ogrenci menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
	};
    void fileogrenciekle (char ad[30], char soyad[20],int no,char bolum[100],int sinif,int id){
        FILE *fogrenciekle = fopen("ogrenci.txt", "a");
        fprintf(fogrenciekle,"%s %s %d %s %d %d\n", ad, soyad, no, bolum, sinif, id);
        fclose(fogrenciekle);
        fileogrencioku ();                                 //ogrenci struct degerlerini ve ogrsayisi guncelleme
    }
//**************************************************struct i guncel tutma ve başlangicta txtyi struct aktarma******************************
    void fileogrencioku (){
    	struct ogrenci *optr=&ogr[0];
        FILE *fogrenci;
		if ((fogrenci = fopen("ogrenci.txt", "r")) == NULL)   //dosya yok ise yeni dosya yarat
		{
		fogrenci = fopen("ogrenci.txt", "w");
		}else{
		fogrenci = fopen("ogrenci.txt", "r");
        int i;
        for (i=0;i>-1;i++) {

              if( feof(fogrenci) ) {
              break ;
              }
			 ogrsayisi=i+1;                                     // ogrenci sayisi
             fscanf(fogrenci,"%s %s %d %s %d %d\n", &optr[i].ad, &optr[i].soyad, &optr[i].no, &optr[i].bolum, &optr[i].sinif, &optr[i].id);
        }
        }
        fclose(fogrenci);
    };
    //**************************************************ogrenci bulma**************************************************
    int ogrencibul(int duzensil)
   {
   	struct ogrenci *optr=&ogr[0];
   	int don;
   int arama;
   int kontrol=0,x;
   printf("ARANICAK OGRENCININ NO'SUNU GIRINIZ\n");
   scanf("%d",&arama);

   int f;
   for(f=0;f<=ogrsayisi;f++)
   {
      if(arama==optr[f].no)
      {
          printf("Aranan ogrenci bulunmustur\n");
          if (duzensil==1){
          ogrenciduzenle(f);

          } else if (duzensil==2){
          ogrencisil(f);
          printf("Ogrencinin kaydi silinmistir\n");
		  }

          kontrol++;
      }
   }
    if(kontrol==0)
   {
    printf("Aradiginiz ogrenci bulunamamistir\n");
   }
   	printf("\nOgrenci menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
   };
//**************************************************ogrenci bilgi degistirme**************************************************
   void ogrenciduzenle(int f)
	{
    struct ogrenci *optr=&ogr[0];
    int kontrol=0;
    int k,ogrno,devamduz=1;
    char ch;
        do
	{
		kontrol=0;
        system("cls");
		printf("\n1. Ogrenci adi degistir");
		printf("\n2. Ogrenci soyadi degistir");
		printf("\n3. Ogrenci no degistir");
		printf("\n4. Ogrenci bolum degistir");
		printf("\n5. Ogrenci sinif degistir");
		printf("\n Seciminizi Belirtiniz (1-5) \n");
		scanf("%c",&ch);
    		switch(ch)
		{
		case '1':
			printf("Ogrencinin yeni adini giriniz: \n");
    		scanf("%s",optr[f].ad);
			printf("\n%s isimli ogrencinin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",optr[f].ad);
        	scanf("%d" ,&devamduz);
			break;
	 	case '2':
			printf("%s isimli ogrencinin yeni soyadini giriniz: \n",optr[f].ad);
    		scanf("%s",optr[f].soyad);
			printf("\n%s isimli ogrencinin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",optr[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		case '3':
			printf("%s isimli ogrencinin yeni numarasini giriniz: \n",optr[f].ad);
			scanf("%d" ,&ogrno);
			for(k=ogrsayisi-1;k>=0;k--){
		       if(optr[k].no == ogrno){
		     	printf("Bu numara zaten sisteme kayitlidir.\n");
			    kontrol=1;
			   }
			}
			if (kontrol == 0){
				optr[f].no = ogrno;
			}
			printf("\n%s isimli ogrencinin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",optr[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		case '4':
			printf("%s isimli ogrencinin yeni bolumunu giriniz: \n",optr[f].ad);
 		    scanf("%s",optr[f].bolum);
			printf("\n%s isimli ogrencinin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",optr[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		case '5':
			printf("%s isimli ogrencinin yeni sinifini giriniz: \n",optr[f].ad);
            scanf("%d",&optr[f].sinif);
            printf("\n%s isimli ogrencinin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",optr[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		}
	}while((ch != '8')&&(devamduz == 1));
    fileogrenciduzenle();
	};
//********************************ogrenci silme************************************
	void ogrencisil(int f)
	{
		int i;
		struct ogrenci *ogrencisil=&ogr[0];
		for (f;f<ogrsayisi;f++){
		i=f+1;
		char *adi=&ogrencisil[f].ad;
		char *soyadi=&ogrencisil[f].soyad;
		char *bolumu=&ogrencisil[f].bolum;
		strcpy(adi,ogrencisil[i].ad);
		strcpy(soyadi,ogrencisil[i].soyad);
	    ogrencisil[f].no=ogrencisil[i].no;
		ogrencisil[f].sinif=ogrencisil[i].sinif;
        strcpy(bolumu,ogrencisil[i].bolum);
		ogrencisil[f].id=ogrencisil[i].id;
	    }
        ogrsayisi=ogrsayisi-1;
	    fileogrenciduzenle ();
	}
//*************************************************structdaki verileri sifirlanmis txtye yazma*****************************************************
    void fileogrenciduzenle (){
    	struct ogrenci *optr=&ogr[0];
        FILE *fogrenciduzenle;
        fogrenciduzenle = fopen("ogrenci.txt", "w");
        int i;
		for (i=0;i<ogrsayisi;i++)  {
			fprintf(fogrenciduzenle,"%s %s %d %s %d %d\n", optr[i].ad, optr[i].soyad, optr[i].no, optr[i].bolum, optr[i].sinif, optr[i].id); // struct i tekrar file a yazarak degisikligi saglamis oluyoruz
		}
	      fclose(fogrenciduzenle);
	};
    int ogrencilistele()
    {
     struct ogrenci *optr=&ogr[0];
     int i,don;
     printf("OGRENCI LISTELE\n");
     printf("OGRENCI AD     OGRENCI SOYAD     OGRENCI NO     OGRENCI BOLUM     OGRENCI SINIF\n");
     for(i=0;i<ogrsayisi;i++)
     {
     printf("%-15s%-15s       %-15d%-15s    %-15d\n",optr[i].ad,optr[i].soyad,optr[i].no,optr[i].bolum,optr[i].sinif);
     }
    printf("Ogrenci menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
    };
// **********************************************firma *************************************************************
    int firmaanafonk(){
    	char ch;
		int sonlandir;
		do
	{
		sonlandir=1;
		printf("\n\n  Firma MENU");
		printf("\n\t1. Firma ekleme");
		printf("\n\t2. Firma duzenleme");
		printf("\n\t3. Firma silme");
		printf("\n\t4. Firma listeleme");
		printf("\n\tSeciminizi Belirtiniz (1-4) \n");
		scanf("%s",&ch);
		switch(ch)
		{
		case '1':
			sonlandir=firmaekle();
			break;
		case '2':
		    sonlandir=firmabul(1);
            break;
		case '3':
		    sonlandir=firmabul(2);
            break;
        case '4':
		    sonlandir=firmalistele();
            break;
		}
	}while((ch!='0')&&(sonlandir!=0));
}
    int firmaekle(){
      	struct firma *fptr=&fir[0];
    	int j,k,don,id;
        int vergino;
        char ad[30],alani[50];
		printf("Firmanin adini giriniz: \n");
		scanf("%s",ad);
		printf("%s isimli firmanin calisma alanini giriniz: \n",ad);
		scanf("%s",alani);
		printf("%s isimli firmanin vergi numarasini giriniz: \n",ad);
		scanf("%d",&vergino);
		for(k=firmasayisi-1;k>=0;k--){
			if(fptr[k].vergino == vergino){
				printf("%s isimli firma zaten sisteme kayitlidir.\n",ad);
				return 1;
			}
		}
		id=firmasayisi+1;
	  	filefirmaekle (ad, alani, vergino, id); //firma.txt olusturma ve bilgileri ekleme fonkiyonu
	printf("Firma menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
	}
	void filefirmaekle (char ad[30], char alani[50],int vno,int id){
       FILE *firmaekle = fopen("firma.txt", "a");
       fprintf(firmaekle,"%s %s %d %d\n", ad, alani, vno, id);
       fclose(firmaekle);
       filefirmaoku ();                                 //firma struct degerlerini ve firmasayisi guncelleme

    }
//**************************************************struct i guncel tutma ve başlangicta txtyi struct aktarma******************************
    void filefirmaoku (){
    	struct firma *firmastr=&fir[0];
        FILE *filefirma;
		if ((filefirma = fopen("firma.txt", "r")) == NULL)   //dosya yok ise yeni dosya yarat
		{
		filefirma = fopen("firma.txt", "w");
		}else{
		filefirma = fopen("firma.txt", "r");
        int i;
        for (i=0;i>-1;i++) {
              if( feof(filefirma) ) {
              break ;
              }
			 firmasayisi=i+1;                                     // firma sayisi
             fscanf(filefirma,"%s %s %d %d\n", &firmastr[i].ad, &firmastr[i].calismalani, &firmastr[i].vergino, &firmastr[i].id);
        }
        }

        fclose(filefirma);
    };
//**************************************************ogrenci bulma**************************************************
    int firmabul(int duzensil)
   {
   struct firma *firmastr=&fir[0];
   int don;
   int arama;
   int kontrol=0,x;
   printf("ARANICAK FIRMA NO'SUNU GIRINIZ\n");
   scanf("%d",&arama);
   int f;
   for(f=0;f<=firmasayisi;f++)
   {
      if(arama==firmastr[f].vergino)
      {
          printf("Aranan firma bulunmustur\n");
          if (duzensil==1){
          firmaduzenle(f);
          } else if (duzensil==2){
          firmasil(f);
          printf("Firma basari ile silinmistir\n");
		  }
          kontrol++;
      }
   }
    if(kontrol==0)
   {
    printf("Aradiginiz firma bulunamamistir\n");
   }

   	printf("\nFirma menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
   };
//**************************************************firma bilgi degistirme**************************************************
   void firmaduzenle(int f)
	{
    struct firma *firmaduzenle=&fir[0];
    int k,devamduz=1,vergino;
    char ch;
    int kontrol=0;
       do
	{
	    kontrol=0;
        system("cls");
		printf("\n1. Firma adi degistir.");
		printf("\n2. Firma calisma alani degistir.");
		printf("\n3. Firma vergi no degistir.");
		printf("\nSeciminizi Belirtiniz (1-3) \n");
		scanf("%c",&ch);
    		switch(ch)
		{
		case '1':
			printf("Firma yeni adini giriniz: \n");
    		scanf("%s",firmaduzenle[f].ad);
			printf("\n%s isimli firmanin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",firmaduzenle[f].ad);
        	scanf("%d" ,&devamduz);
			break;
	 	case '2':
			printf("%s isimli firmanin yeni calisma alanini giriniz: \n",firmaduzenle[f].ad);
    		scanf("%s",firmaduzenle[f].calismalani);
			printf("\n%s isimli firmanin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",firmaduzenle[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		case '3':
			printf("%s isimli firmanin yeni vergi numarasini giriniz: \n",firmaduzenle[f].ad);
			scanf("%d" ,&vergino);
			for(k=ogrsayisi-1;k>=0;k--){
		       if(firmaduzenle[k].vergino == vergino){
		     	printf("Bu numara zaten sisteme kayitlidir.\n");
			    kontrol=1;
			   }
			}
			if (kontrol == 0){
				firmaduzenle[f].vergino = vergino;
			}
			printf("\n%s isimli firmanin baska bir bilgisini degistirmek icin 1'e basiniz, devam etmek icin 0'a basiniz\n",firmaduzenle[f].ad);
        	scanf("%d" ,&devamduz);
			break;
		}
	}while((ch != '8')&&(devamduz == 1));
    filefirmaduzenle();
	};
//*************************************************firma silme************************************
	void firmasil(int f)
	{
		int i;
		struct firma *firmasil=&fir[0];
		for (f;f<ogrsayisi;f++){
		i=f+1;
		char *adi=&firmasil[f].ad;
		char *alani=&firmasil[f].calismalani;
		strcpy(adi,firmasil[i].ad);
		strcpy(alani,firmasil[i].calismalani);
	    firmasil[f].vergino=firmasil[i].vergino;
		firmasil[f].id=firmasil[i].id;
	    }
        firmasayisi=firmasayisi-1;
	    filefirmaduzenle ();
	}
//**************************************************structdaki verileri sifirlanmis txtye yazma*****************************************************
    void filefirmaduzenle (){
    	struct firma *firmatxtyaz=&fir[0];
        FILE *ffirmaduzenle;
        ffirmaduzenle = fopen("firma.txt", "w");
        int i;
		for (i=0;i<firmasayisi;i++)  {
			fprintf(ffirmaduzenle,"%s %s %d %d\n", firmatxtyaz[i].ad, firmatxtyaz[i].calismalani, firmatxtyaz[i].vergino, firmatxtyaz[i].id); // struct i tekrar file a yazarak degisikligi saglamis oluyoruz
		}
	      fclose(ffirmaduzenle);
	};
//*******************************************firma listele*******************************************
    int firmalistele()
    {
     struct firma *firmastr=&fir[0];
     int i,don;
     printf("FIRMA LISTELE\n");
     printf("FIRMA AD     FIRMA CALISMA ALANI     FIRMA VERGINO    \n");
     for(i=0;i<firmasayisi;i++)
     {
     printf("%-15s%-15s       %-15d    \n",firmastr[i].ad,firmastr[i].calismalani,firmastr[i].vergino);
     }
    printf("Firma menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
    };
//***************************************************STAJ****************************************************************
    int stajanafonk()
    {
    	char ch;
		int sonlandir;
		do
	{
		sonlandir=1;
		printf("\n\n  Staj MENU");
		printf("\n\t1. Staj ekleme");
		printf("\n\t2. Tamamlanmis stajlar");
		printf("\n\t3. Tamamlanmamis stajlar");
		printf("\n\tSeciminizi Belirtiniz (1-4) \n");
		scanf("%s",&ch);
		switch(ch)
		{
		case '1':
			sonlandir=stajekle();
			break;
		case '2':
		    sonlandir=stajdurumu(1);
            break;
		case '3':
		    sonlandir=stajdurumu(2);
            break;
		}
	}while((ch!='0')&&(sonlandir!=0));
	}
//************************************************staj ekleme***************************************
	int stajekle()
	{
		struct firma *firmastr=&fir[0];
		struct ogrenci *ogrstr=&ogr[0];
    	int don;
    	int ogrid,firid;
        char turu[50];
        int kontrol=0,ogrno,f,firno;
		printf("Ogrenci no giriniz: \n");
		scanf("%d",&ogrno);
	   for(f=0;f<=ogrsayisi;f++)
   	{
    	if(ogrno==ogrstr[f].no)
    	{
    	  ogrid = ogrstr[f].id;
          printf("%-15s%-15s       %-15d%-15s    %-15d\n",ogrstr[f].ad,ogrstr[f].soyad,ogrstr[f].no,ogrstr[f].bolum,ogrstr[f].sinif);
          kontrol=1;
      	}
    }
    if(kontrol==0)
   {
    printf("Aradiginiz ogrenci bulunamamistir\n");
    return 1;
   }
   		printf("Firma vergi no giriniz: \n");
		scanf("%d",&firno);
	   for(f=0;f<=firmasayisi;f++)
   	{
    	if(firno==firmastr[f].vergino)
    	{
    		firid = firmastr[f].id;
     		printf("%-15s%-15s       %-15d    \n",firmastr[f].ad,firmastr[f].calismalani,firmastr[f].vergino);
          kontrol=2;
      	}
    }
    if(kontrol==1)
   {
    printf("Aradiginiz firma bulunamamistir\n");
    return 1;
   }
    printf("Staj turunu giriniz: \n");
    scanf("%s",turu);
    char string1[256] , string2[256];
	printf(" staj baslangic tarihi giriniz : ");
	scanf("%s", string1);
	printf(" staj bitis tarihi giriniz : ");
	scanf("%s", string2);
	int digit, k = 0, j = 1, m = 0, gun1 = 0, ay1 = 0, yil1 = 0 , gun2 = 0, ay2 = 0, yil2 = 0,i;
	 for( i = strlen(string1)-1 ; i>=0 ; i--)
	 {
		digit = string1[i];
		if(digit == 46){

			if(k == 4){
				yil1 = m;
				m = 0;
				j = 1;
				continue;
			}
			if(k == 6){
				ay1 = m;
				m = 0;
				j = 1;
				continue;
			}
		}
		digit = digit - 48;
		m = m + (digit * j);
		j = j * 10;
		k++;
		if(k == 8){
				gun1 = m;
				m = 0;
				j = 1;
				continue;
			}
	}
//**********************************************************************tarih aralik hesabi************************************************************************************************
    digit = 0 ; k = 0 ; j = 1 ; m = 0 ; gun2 = 0 ; ay2 = 0 ; yil2 = 0;
    for(i = strlen(string2)-1 ; i>=0 ; i--)
    {
		digit = string2[i];
		if(digit == 46){

			if(k == 4){
				yil2 = m;
				m = 0;
				j = 1;
				continue;
			}
			if(k == 6){
				ay2 = m;
				m = 0;
				j = 1;
				continue;
			}
		}
		digit = digit - 48;
		m = m + (digit * j);
		j = j * 10;
		k++;
		if(k == 8){
				gun2 = m;
				m = 0;
				j = 1;
				continue;
			}
	}
//****************************************staj hafta hesaplama***************************************
	int gun3 = 0, ay3 = 0, hafta = 0;
	if(ay1 == ay2){
		gun3 = gun2 - gun1 ;
		gun3 = gun3 - gun3%7;
		hafta = gun3/7;
	}
	if(yil1 == yil2){
	hafta = hafta + ( ay2 - ay1 - 1 ) * 4;
	hafta = hafta + ( ( 30 - gun1 + gun2 )-( 30 - gun1 + gun2 ) % 7 ) / 7 ;
	}
	if(yil1 > yil2){
		hafta = hafta + ay2 * 4 + ( (gun2 - ( gun2 % 7 ) ) / 7);
		hafta = hafta + ((12 - ay1 ) * 4) - ( ( gun1 - ( gun1 % 7 ) ) / 7 );
	}
	printf("staj hafta sayiniz : %d",hafta);
  	filestajekle (string1, string2, hafta, turu, ogrid, firid); //firma.txt olusturma ve bilgileri ekleme fonkiyonu
	printf("Staj menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
	}
    void filestajekle (char bastarih[20], char bitistarih[20],int stajhafta,char stajturu[50], int ogrno, int firmano )
    {
       FILE *stajekle = fopen("staj.txt", "a");
       fprintf(stajekle,"%s %s %d %s %d %d\n", bastarih, bitistarih, stajhafta, stajturu, ogrno, firmano);
       fclose(stajekle);
       filestajoku ();                            //firma struct degerlerini ve firmasayisi guncelleme
    }
//**************************************************struct i guncel tutma ve başlangicta txtyi struct aktarma******************************
    void filestajoku (){

    	struct staj *stajptr=&stj[0];
        FILE *filestaj;
		if ((filestaj = fopen("staj.txt", "r")) == NULL)   //dosya yok ise yeni dosya yarat
		{
		filestaj = fopen("staj.txt", "w");
		}else{
		filestaj = fopen("staj.txt", "r");
        int i;
        for (i=0;i>-1;i++)
            {
              if( feof(filestaj) )
              {
              break ;
              }
			 stajsayisi=i+1;                                     // staj sayisi
             fscanf(filestaj,"%s %s %d %s %d %d\n", &stajptr[i].baslangic, &stajptr[i].bitis, &stajptr[i].haftasayisi, &stajptr[i].stajturu, &stajptr[i].ogrid, &stajptr[i].firid);
        }
        }
        fclose(filestaj);
    };
//************************************************staj listaleme********************************************************
        int stajdurumu (int durum){
        int i,k,don;
    	struct staj *stajptr=&stj[0];
    	struct ogrenci *ogrptr=&ogr[0];
    	struct firma *firmaptr=&fir[0];
    	int donanimhafta;
    	int yazilimhafta;
    	int toplamhafta;
    	char tur[50]="donanim";
    	char turyaz[50]="yazilim";
	for (k=0;k<ogrsayisi;k++){
			donanimhafta=0;
			yazilimhafta=0;
			toplamhafta=0;
    	for (i=0;i<stajsayisi;i++){
    		char adi[50];
    		strcpy(adi,stajptr[i].stajturu);
    	if (stajptr[i].ogrid==ogrptr[k].id){
    		if(*adi==*tur){
    		donanimhafta+=&stajptr[i].haftasayisi;
			}else if(*adi==*turyaz){
    		yazilimhafta+=&stajptr[i].haftasayisi;
			}
		}
        }
     toplamhafta=donanimhafta+yazilimhafta;
     if((toplamhafta>=12)&&(donanimhafta>=2)&&(yazilimhafta>=4)){
     ogrptr[k].stajdurumu=1;
     }else{
     ogrptr[k].stajdurumu=2;
	 }
    }
      if (durum==1){
      	bitmisstaj();
	  } else if (durum==2)
	  {
	  	yarimstaj();
	  }
	printf("Staj menusune donmek icin 1'e basiniz. Ana menuye donmek icin 0'a basiniz: \n");
	scanf("%d" ,&don);
	return don;
    };
    void bitmisstaj(){
     struct ogrenci *optr=&ogr[0];
     int i,don;
     printf("STAJI BITMIS OGRENCILER \n");
     printf("OGRENCI AD     OGRENCI SOYAD     OGRENCI NO     OGRENCI BOLUM     OGRENCI SINIF\n");
     for(i=0;i<ogrsayisi;i++)
        {
     	if(optr[i].stajdurumu==1){
    	 printf("%-15s%-15s       %-15d%-15s    %-15d\n",optr[i].ad,optr[i].soyad,optr[i].no,optr[i].bolum,optr[i].sinif);
            }
        }
    };
	void yarimstaj(){
	struct ogrenci *optr=&ogr[0];
     int i,don;
     printf("STAJI BITMEMIS OGRENCILER \n");
     printf("OGRENCI AD     OGRENCI SOYAD     OGRENCI NO     OGRENCI BOLUM     OGRENCI SINIF\n");
     for(i=0;i<ogrsayisi;i++)
     {
     	 if(optr[i].stajdurumu==2){
    	 printf("%-15s%-15s       %-15d%-15s    %-15d\n",optr[i].ad,optr[i].soyad,optr[i].no,optr[i].bolum,optr[i].sinif);
            }
        }
	};
