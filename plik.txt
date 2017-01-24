#include <allegro.h>
#include<fstream.h>


using namespace std;
volatile long speed = 0;
int x=1,y=1;
int rekord;
int punkty=0;
int mx=0,my=0,mklikniecie=0;
int odczyt()
{
    ifstream wejscie("rekord.txt");
    wejscie>>rekord;
}

void kursor()
{
     if( mx != mouse_x || my != mouse_y || mklikniecie != mouse_b )
    {
        mx = mouse_x;
        my = mouse_y;
        mklikniecie = mouse_b;
    }
 }


  

int odmierzanie(int czas)
{
    
    if(clock()-czas>30000)
    {
    if(punkty>=rekord)
    {
    ofstream zapisze ("rekord.txt");
    zapisze<<punkty;
    zapisze.close();
    }
    set_gfx_mode( GFX_TEXT, 0, 0, 0, 0 );
    allegro_message( "Koniec czasu :D" );
    allegro_exit();
    return 0;
}
}

int nieobraz(BITMAP*postac=NULL,BITMAP*potworek=NULL,BITMAP*tapeta=NULL,SAMPLE * dzwiek = NULL)
{
     if( !postac )
{
    set_gfx_mode( GFX_TEXT, 0, 0, 0, 0 );
    allegro_message( "nie moge zaladowac obrazka postac" );
    allegro_exit();
    return 0;
}
if( !potworek )
{
    set_gfx_mode( GFX_TEXT, 0, 0, 0, 0 );
    allegro_message( "nie moge zaladowac obrazka potworek" );
    allegro_exit();
    return 0;
}

if( !tapeta )
{
    set_gfx_mode( GFX_TEXT, 0, 0, 0, 0 );
    allegro_message( "nie moge zaladowac obrazka tapeta" );
    allegro_exit();
    return 0;
}
/*if( !dzwiek )
{
    set_gfx_mode( GFX_TEXT, 0, 0, 0, 0 );
    allegro_message( "nie moga zaladowac dzwieku !" );
    allegro_exit();
    return 0;*/
}

 
 
 void increment_speed()
{
    speed++;
}
END_OF_FUNCTION( increment_speed );
LOCK_VARIABLE( speed );
LOCK_FUNCTION( increment_speed );
 

int main()
{   odczyt();
    
    int czas;
    int lokacja_x[100];
    int lokacja_y[100];
    srand(time(NULL));
    allegro_init();
    install_keyboard(); 
    install_mouse();
    //install_sound( DIGI_AUTODETECT, MIDI_AUTODETECT, "" );
    set_volume( 255, 255 );
    set_gfx_mode( GFX_AUTODETECT_WINDOWED, 640, 400, 0, 0 );
    clear_to_color( screen, makecol( 128, 0, 128 ) );
    set_color_depth( 16 );
    BITMAP * bufor=NULL;
    //SAMPLE * dzwiek = NULL;
    BITMAP * postac =NULL;
    BITMAP * potworek =NULL;
    BITMAP * tapeta =NULL;
    install_timer();
    install_int_ex( increment_speed, BPS_TO_TIMER( 40 ) );
    bufor=create_bitmap(640,400);
    postac = load_bmp( "postac.bmp", default_palette );
    potworek = load_bmp( "potworek.bmp", default_palette );
    tapeta = load_bmp( "tapeta.bmp", default_palette );
    //dzwiek = load_sample( "muzyczka.wav" );
    show_mouse( screen );
    unscare_mouse();  
    
    BITMAP* menu=NULL;
    BITMAP* start=NULL;
    BITMAP* wyjscie=NULL;
    BITMAP* tytul=NULL;
    menu = load_bmp( "menu.bmp", default_palette );
    start = load_bmp( "start.bmp", default_palette );
    wyjscie = load_bmp( "wyjscie.bmp", default_palette );
    tytul = load_bmp( "tytul.bmp", default_palette );
    //-----------
        while( !key[ KEY_ESC ] )
{
    kursor();
    clear_to_color( bufor, makecol( 150, 150, 150 ) );
    blit( menu, bufor, 0, 0, 0, 0, menu->w, menu->h );
    masked_blit( start, bufor, 0, 0, 132, 242, start->w, start->h );
    masked_blit( wyjscie, bufor, 0, 0, 472, 242, wyjscie->w, wyjscie->h );
    masked_blit( tytul, bufor, 0, 0, 35, 86, tytul->w, tytul->h );
    if(mklikniecie==1)
    {
    if((mx<=200)&&(my>=222))
    break;
    if((mx>=400)&&(my>=222))
    {
    destroy_bitmap(menu);
    destroy_bitmap(start);
    destroy_bitmap(wyjscie);
    destroy_bitmap(tytul);
    destroy_bitmap(postac);
    destroy_bitmap(potworek);
    remove_int( increment_speed );
    destroy_bitmap(bufor);
    allegro_exit();
    return 0;
    };
}
    blit( bufor, screen, 0, 0, 0, 0, 640, 400 );
}
    //-----------

nieobraz(postac,potworek,tapeta); 

int punkty1=0;
int postac_x=100,postac_y=100;


while( !key[ KEY_ESC ] )
{
       while( speed > 0 )
{
    if( key[ KEY_LEFT ] )
    {   
        postac_x--;
        if(postac_x<1) 
        postac_x=1;
        
    }
    if( key[ KEY_RIGHT ] )
    {   
        postac_x++;
        if(postac_x>607) 
        postac_x=607;
    } 
    
    
    if( key[ KEY_UP ] ) 
    {
    postac_y--;
    if (postac_y<1)
    postac_y=1;
    }

    if( key[ KEY_DOWN ] )
    {
    postac_y++;
    if (postac_y++>366)
    postac_y=366; 
    }  
    
    if ((postac_x-20==x) && (postac_y-20<=y) || (postac_x+20==x) && (postac_y+20<=y) || (postac_x==x) && (postac_y<=y) )
    {
    punkty++; punkty1=1;
    x=rand()%300+1;
      y=rand()%300+10;
    } else punkty1=0;
        speed--;
}
   odmierzanie(czas);
   //play_sample( dzwiek, 255, 127, 1000, 1 );
    clear_to_color( bufor, makecol( 150, 150, 150 ) );
    blit( tapeta, bufor, 0, 0, 0, 0, tapeta->w, tapeta->h );
    textprintf_ex(bufor,font,40,380,makecol(255,255,255),-1,"X:");
    textprintf_ex(bufor,font,60,380,makecol(255,255,255),-1,"%d",postac_x);
    textprintf_ex(bufor,font,90,380,makecol(255,255,255),-1,"Y:");
    textprintf_ex(bufor,font,110,380,makecol(255,255,255),-1,"%d",postac_y);
    textprintf_ex(bufor,font,500,1,makecol(255,255,255),-1,"Punkty:");
    textprintf_ex(bufor,font,580,1,makecol(255,255,255),-1,"%d",punkty);
    textprintf_ex(bufor,font,500,10,makecol(255,255,255),-1,"Rekord:");
    textprintf_ex(bufor,font,580,10,makecol(255,255,255),-1,"%d",rekord);
    textprintf_ex(bufor,font,540,20,makecol(255,255,255),-1,"Czas:");
    textprintf_ex(bufor,font,580,20,makecol(255,255,255),-1,"%d",clock()-(czas<30000));
    

    
    if(punkty1=1)
    {
    masked_blit( potworek, bufor, 0, 0, x, y, potworek->w, potworek->h );
    }
    
    blit( postac, bufor, 0, 0, postac_x, postac_y, postac->w, postac->h );
    blit( bufor, screen, 0, 0, 0, 0, 640, 400 );
   
    
}


destroy_bitmap(postac);
destroy_bitmap(potworek);
remove_int( increment_speed );
destroy_bitmap(bufor);
//stop_sample( dzwiek );
destroy_bitmap(menu);
destroy_bitmap(start);
destroy_bitmap(wyjscie);
destroy_bitmap(tytul);
allegro_exit();
return 0;
}
END_OF_MAIN();

