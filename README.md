#include "iGraphics.h"
#include"bitmap_loader.h"


void drawHomepage();
void drawStartPage();
void drawAboutPage();
void drawInstructionPage();
void drawScorePage();
void drawEasyPage();
void drawMediumPage();
void drawHardPage();
void gameover();


void startButtonClickHandler();
void aboutButtonClickHandler();
void instructionButtonClickHandler();
void scoreButtonClickHandler();
void backButtonClickHandler();
void easyButtonClickHandler();
void mediumButtonClickHandler();
void hardButtonClickHandler();


int startButtonClick = 0;
int aboutButtonClick = 0;
int instructionButtonClick = 0;

int homePage = 1;
int startPage = 0;
int aboutPage = 0;
int instructionPage = 0;
int scorePage = 0;
int easyPage = 0;
int mediumPage = 0;
int hardPage = 0;

int playerX = 470, playerY = 0; // CarHero
float x = 400, y = 601; //v1
float a = 600, b = 601; //v2
float c = 550, d = 601; //v3

//collision
void collision()
{
	// v1
	if ((playerX + 63 >= x && playerX <= x + 63) && (playerY + 124 >= y && playerY <= y + 124))
	{
		easyPage = 0;
		mediumPage = 0;
		hardPage = 0;
		homePage = 2;
	}
	else if ((playerX + 63 >= a && playerX <= a + 63) && (playerY + 124 >= b && playerY <= b + 124))
	{
		easyPage = 0;
		mediumPage = 0;
		hardPage = 0;
		homePage = 2;
	}
	else if ((playerX + 63 >= c && playerX <= c + 63) && (playerY + 124 >= d && playerY <= d + 124))
	{
		easyPage = 0;
		mediumPage = 0;
		hardPage = 0;
		homePage = 2;
	}
}




// For rendering
	int totalImageCount = 30;
	int increment = 20;
	char backgroundImagePath[200][250] = { "pic\\01.bmp", "pic\\02.bmp", "pic\\03.bmp", "pic\\04.bmp", "pic\\05.bmp", "pic\\06.bmp", "pic\\07.bmp", 
	"pic\\08.bmp", "pic\\09.bmp", "pic\\10.bmp","pic//11.bmp", "pic\\12.bmp", "pic\\13.bmp", "pic\\14.bmp", "pic\\15.bmp", "pic\\16.bmp", "pic\\17.bmp", 
	"pic\\18.bmp", "pic\\19.bmp", "pic\\20.bmp", "pic\\21.bmp", "pic\\22.bmp", "pic\\23.bmp", "pic\\24.bmp", "pic\\25.bmp", "pic\\26.bmp", "pic\\27.bmp",
	"pic\\28.bmp", "pic\\29.bmp", "pic\\30.bmp" };
	int imgPosition[100];
	int backgroundY = 0;
	int backgroundImagePathIndex = 0;

void inatializeImagePosition()
{
		int i, j;
		for (i = 0, j = 0; i < totalImageCount; i++){
			imgPosition[i] = j;
			j = j + increment;
		}
}

void moveBackground()
{
	for (int i = 0; i < totalImageCount; i++)
	{
		imgPosition[i] = imgPosition[i] - increment;
	}
	for (int i = 0; i < totalImageCount; i++)
	{
		if (imgPosition[i] < 0)
		{
			imgPosition[i] = 600 - increment;
		}
	}
}



void rendering(){
	for (int i = 0; i < totalImageCount; i++)
	{
		iShowBMP2(0, imgPosition[i], backgroundImagePath[i], 0);
	}
}



int c1Y = 50;
int c2Y = 300;
int c3Y = 500;
int c1X = 300;
int c2X = 500;
int c3X = 700;
int c1Xspeed = 0;
int c2Xspeed = 0;
int c3Xspeed = 0;
int coinScore = 0;

int second = 0;
int score = 0;


// For coincollision

void coinCollision()
{
	// Coin 1
	if ((playerX + 63 >= c1X && playerX <= c1X + 28) && (playerY + 124 >= c1Y && playerY <= c1Y + 26))
	{
		coinScore += 5;
		c1Y = 600;  // Reset coin to the top
		c1X = 300 + rand() % 400; // Randomize X position
	}
	// Coin 2
	else if ((playerX + 63 >= c2X && playerX <= c2X + 28) && (playerY + 124 >= c2Y && playerY <= c2Y + 26))
	{
		coinScore += 5;
		c2Y = 600;
		c2X = 300 + rand() % 400;
	}
	// Coin 3
	else if ((playerX + 63 >= c3X && playerX <= c3X + 28) && (playerY + 124 >= c3Y && playerY <= c3Y + 26))
	{
		coinScore += 5;
		c3Y = 600;
		c3X = 300 + rand() % 400;
	}


	score = second + coinScore;


}



char sec[10000];
int minute = 0;
int hour = 0;
char hr[10000];
int totalSecond = 0;
char point[10000];



void changeTimer()
{
	if (easyPage == 1 || mediumPage == 1 || hardPage == 1)
	{
		totalSecond = (second++) + (60 * minute) + (60 * 60 * hour);
		score = score + second;
	}
}

void drawTimer()
{
	if (easyPage == 1 || mediumPage == 1 || hardPage == 1)
	{
		iSetColor(255, 204, 203);
		iFilledRectangle(790, 510, 200, 40);

		iSetColor(255, 0, 0);
		sprintf_s(sec, "%d", second);
		iText(940, 520, sec, GLUT_BITMAP_TIMES_ROMAN_24);

		iText(850, 520, "Timer", GLUT_BITMAP_TIMES_ROMAN_24);

		iSetColor(255, 204, 203);
		iFilledRectangle(850, 460, 100, 50);
		iSetColor(0, 0, 0);
		iText(855, 495, "SCORE", GLUT_BITMAP_TIMES_ROMAN_24);
		sprintf_s(point, "%d", score);
		iText(855, 468, point, GLUT_BITMAP_TIMES_ROMAN_24);
	}
}

void newHighscore();
void readScore();
int len = 0;
char str1[100];
bool newscore = true;

struct hscore {
	char name[30];
	int score = 0;
} high_score[5];

void readScore()
{
	FILE* fp;
	fp = fopen("score.txt", "r");
	
	char showName[5][30], showScore[5][5];

	for (int i = 0; i < 5; i++) {
		fscanf(fp, "%s %d\n", high_score[i], &high_score[i].score);
	}

	for (int i = 0; i < 5; i++) {
		sprintf(showName[i], "%s", high_score[i].name);
		sprintf(showScore[i], "%d", high_score[i].score);
		iSetColor(0, 255, 255);
		iText(400, 550 - 50 * i, showName[i], GLUT_BITMAP_HELVETICA_18);
		iText(500, 550 - 50 * i, showScore[i], GLUT_BITMAP_HELVETICA_18);
	}
	fclose(fp);
} 

void newHighscore()
{
	FILE *fp;
	fp = fopen("Score.txt", "w");

	for (int i = 0; i < 5; i++) {
		fscanf(fp, "%s %d\n", high_score[i].name, &high_score[i].score);
	}
	int t;//temp score for s
	char n[30];//temp name for s
	if (newscore)
	{
		for (int i = 0; i < 5; i++) 
		{
			if (high_score[i].score < score)
			{
				high_score[4].score = score;
				strcpy(high_score[4].name, str1);

				
				for (int j = 0; j < 5; j++) {//fix the condition here
					for (int k = 5; k > j; k--) {  
						if (high_score[k].score > high_score[k - 1].score) {
							// Sorting logic
							int t = high_score[k - 1].score;
							high_score[k - 1].score = high_score[k].score;
							high_score[k].score = t;

							
							char n[50]; // declare  n here 
							strcpy(n, high_score[k - 1].name);
							strcpy(high_score[k - 1].name, high_score[k].name);
							strcpy(high_score[k].name, n);
						}
					}
				}

				// Write the sorted scores to the file
				FILE* fp = fopen("Score.txt", "w");
				for (int i = 0; i < 5; i++) {
					fprintf(fp, "%s %d\n", high_score[i].name, high_score[i].score);
				}
				fclose(fp);

				newscore = false;  // Reset newscore
				break;  // Exit the loop since sorting is done
			}
		}
	}
}

void showChar()
{
	iSetColor(255, 0, 0);
	iText(400, 500, "Enter your name :", GLUT_BITMAP_HELVETICA_18);
	iRectangle(495, 450, 160, 30);
	iText(500, 460, str1, GLUT_BITMAP_HELVETICA_18);  // Fixed x position
}

void takeinput(unsigned key)
{
	if (key == '\r')  // Enter key
	{
		homePage = 1;  
		newHighscore();  
	}
	else if (key == '\b')  // Backspace key
	{
		if (len > 0)
			
		 len--;
		str1[len] = '\0';
	}
	else{
		str1[len] = key;
		len++;
		if (len > 15)
			len = 15;
		str1[len] = '\0';
	}
	
}




bool musicOn = true;

void iDraw()
{
	iClear();
	if (homePage == 1)
	{
		drawHomepage();
	}
	else if (startPage == 1)
	{
		drawStartPage();
	}
	else if (aboutPage == 1)
	{
		drawAboutPage();
	}
	else if (instructionPage == 1)
	{
		drawInstructionPage();
	}
	else if (scorePage == 1)
	{
		drawScorePage();
	}
	else if (easyPage == 1)
	{
		drawEasyPage();
	}
	else if (mediumPage == 1)
	{
		drawMediumPage();
	}
	else if (hardPage == 1)
	{
		drawHardPage();
	}
	else if (homePage == 2)
	{
		gameover();
		showChar();
	}
	
}

void iMouseMove(int mx, int my)
{

}

void iPassiveMouseMove(int mx, int my)
{

}

void iMouse(int button, int state, int mx, int my)
{
	if (button == GLUT_LEFT_BUTTON && state == GLUT_DOWN)
	{
		printf("x = %d y = %d\n", mx, my);

		if (homePage == 1 && (mx >= 371 && mx <= 617) && (my >= 505 && my <= 580))
		{
			startButtonClickHandler();
		}
		else if (homePage == 1 && (mx >= 371 && mx <= 618) && (my >= 403 && my <= 480))
		{
			aboutButtonClickHandler();
		}
		else if (homePage == 1 && (mx >= 372 && mx <= 617) && (my >= 303 && my <= 379))
		{
			instructionButtonClickHandler();
		}
		else if (homePage == 1 && (mx >= 372 && mx <= 617) && (my >= 204 && my <= 278))
		{
			scoreButtonClickHandler();
		}
		else if (startPage == 1 | aboutPage == 1 | instructionPage == 1 | scorePage == 1 && (mx >= 21 && mx <= 167) && (my >= 480 && my <= 600))
		{
			backButtonClickHandler();
		}
		else if (startPage == 1 && (mx >= 371 && mx <= 617) && (my >= 505 && my <= 580))
		{
			easyButtonClickHandler();
		}
		else if (startPage == 1 && (mx >= 371 && mx <= 618) && (my >= 403 && my <= 480))
		{
			mediumButtonClickHandler();
		}
		else if (startPage == 1 && (mx >= 372 && mx <= 617) && (my >= 303 && my <= 379))
		{
			hardButtonClickHandler();
		}

	}

	if (button == GLUT_RIGHT_BUTTON && state == GLUT_DOWN)
	{
		// Add logic for right button click
	}
}

void iKeyboard(unsigned char key)
{
	if (homePage == 2) // If game is over, take name input
	{
		takeinput(key);
	}
	if (key == 'q')  // Quit game when 'q' is pressed
	{
		exit(0);
	}
}


void iSpecialKeyboard(unsigned char key)
{
	if (key == GLUT_KEY_RIGHT)
	{
		// Logic for right arrow key
		playerX = playerX + 15;
		if (playerX > 700)
		{
			playerX = 700;
		}
	}
	if (key == GLUT_KEY_LEFT)
	{
		// Logic for left arrow key
		playerX = playerX - 15;
		if (playerX < 300)
		{
			playerX = 300;
		}
	}
	if (key == GLUT_KEY_UP)
	{
		// Logic for Home key
		playerY = playerY + 10;
		if (playerY > 486)
		{
			playerY = 486;
		}
	}
	if (key == GLUT_KEY_DOWN)
	{
		// Logic for Home key
		playerY = playerY - 10;
		if (playerY < 0)
		{
			playerY = 0;
		}
	}
	if (key == GLUT_KEY_F1)
	{
		if (musicOn){
			musicOn = false;
			PlaySound(0, 0, 0);
		}
		else
		{
			musicOn = true;
			PlaySound("CarMusicOn.wav", NULL, SND_LOOP | SND_ASYNC);
		}
	}
}

void drawHomepage()
{
	iSetColor(128, 128, 128);
	iFilledRectangle(0, 0, 1000, 600); // Background
	iShowBMP2(0, 0, "image\\car.bmp", 0); // Background image

	iShowBMP2(370, 500, "image\\button 1.bmp", 0);
	iShowBMP2(370, 400, "image\\button 2.bmp", 0);
	iShowBMP2(370, 300, "image\\button 3.bmp", 0);
	iShowBMP2(370, 200, "image\\button 4.bmp", 0);

	iSetColor(255, 255, 255);
	iText(450, 520, "Start", GLUT_BITMAP_TIMES_ROMAN_24);
	iText(450, 420, "About Us", GLUT_BITMAP_TIMES_ROMAN_24);
	iText(450, 320, "Instruction", GLUT_BITMAP_TIMES_ROMAN_24);
	iText(450, 220, "Score", GLUT_BITMAP_TIMES_ROMAN_24);
}

void drawStartPage()
{
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\Car1.bmp", 0);

	iShowBMP2(370, 500, "image\\button 1.bmp", 0);
	iText(450, 520, "Easy", GLUT_BITMAP_TIMES_ROMAN_24);
	iShowBMP2(370, 400, "image\\button 2.bmp", 0);
	iText(450, 420, "Medium", GLUT_BITMAP_TIMES_ROMAN_24);
	iShowBMP2(370, 300, "image\\button 3.bmp", 0);
	iText(450, 320, "Hard", GLUT_BITMAP_TIMES_ROMAN_24);

	iShowBMP2(20, 475, "image\\Back2.bmp", 0);
}

void drawAboutPage()
{
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\AboutUss.bmp", 0);
	iShowBMP2(20, 475, "image\\Back2.bmp", 0);
}

void drawInstructionPage()
{
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\instruction.bmp", 0);
	iShowBMP2(20, 475, "image\\Back2.bmp", 0);
}
void drawScorePage(){
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\Score.bmp", 0);
	iShowBMP2(20, 475, "image\\Back2.bmp", 0);

	readScore();
	homePage = 2;
}
void drawEasyPage(){
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\CarRoad3.bmp", 0);


	rendering();

	
	iShowBMP2(playerX, playerY, "image\\carHero.bmp", 0);
	iShowBMP2(x, y, "image\\v1.bmp", 0);
	iShowBMP2(a, b, "image\\v2.bmp", 0);
	iShowBMP2(c, d, "image\\v3.bmp", 0);


	//v1
	y = y - 1.5;

	if (y <= 0)
	{
		y = 601;
		x = 310 + rand() % 350;
	} collision();
	// v2
	b = b - 2;

	if (b <= 0)
	{
		b = 601;
		a = 300 + rand() % 360;
	} collision();
	// v3
	d = d - 2.5;

	if (d <= 0)
	{
		d = 601;
		c = 300 + rand() % 360;
	} collision();


	iShowBMP2(c1X, c1Y, "image\\c1.bmp", 0);
	iShowBMP2(c2X, c2Y, "image\\c2.bmp", 0);
	iShowBMP2(c1X, c3Y, "image\\c3.bmp", 0);

	c1Y = c1Y - 7;
	if (c1Y <= 0){
		c1Y = 600;
		c1X = 400 + rand() % 350;
	}coinCollision();

	c2Y = c2Y - 7.5;
	if (c2Y <= 0){
		c2Y = 600;
		c2X = 360 + rand() % 300;
	}coinCollision();

	c3Y = c3Y - 8;
	if (c3Y <= 0){
		c3Y = 600;
		c3X = 310 + rand() % 350;
	}coinCollision();


	drawTimer();

}

	

void drawMediumPage(){
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\CarRoad3.bmp", 0);

	rendering();


	iShowBMP2(playerX, playerY, "image\\carHero.bmp", 0);
	iShowBMP2(x, y, "image\\v1.bmp", 0);
	iShowBMP2(a, b, "image\\v2.bmp", 0);
	iShowBMP2(c, d, "image\\v3.bmp", 0);


	//v1
	y = y - 3;

	if (y <= 0)
	{
		y = 601;
		x = 310 + rand() % 350;
	} collision();
	// v2
	b = b - 3.5;

	if (b <= 0)
	{
		b = 601;
		a = 300 + rand() % 360;
	} collision();
	// v3
	d = d - 4;

	if (d <= 0)
	{
		d = 601;
		c = 300 + rand() % 360;
	} collision();

	iShowBMP2(c1X, c1Y, "image\\c1.bmp", 0);
	iShowBMP2(c2X, c2Y, "image\\c2.bmp", 0);
	iShowBMP2(c1X, c3Y, "image\\c3.bmp", 0);

	c1Y = c1Y - 7;
	if (c1Y <= 0){
		c1Y = 600;
		c1X = 400 + rand() % 350;
	}coinCollision();

	c2Y = c2Y - 7.5;
	if (c2Y <= 0){
		c2Y = 600;
		c2X = 360 + rand() % 300;
	}coinCollision();

	c3Y = c3Y - 8;
	if (c3Y <= 0){
		c3Y = 600;
		c3X = 310 + rand() % 350;
	}coinCollision();

	drawTimer();

}
void drawHardPage(){
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\CarRoad3.bmp", 0);

	rendering();


	iShowBMP2(playerX, playerY, "image\\carHero.bmp", 0);
	iShowBMP2(x, y, "image\\v1.bmp", 0);
	iShowBMP2(a, b, "image\\v2.bmp", 0);
	iShowBMP2(c, d, "image\\v3.bmp", 0);


	//v1
	y = y - 4.5;

	if (y <= 0)
	{
		y = 601;
		x = 310 + rand() % 350;
	} collision();
	// v2
	b = b - 5;

	if (b <= 0)
	{
		b = 601;
		a = 300 + rand() % 360;
	} collision();
	// v3
	d = d - 5.5;

	if (d <= 0)
	{
		d = 601;
		c = 300 + rand() % 360;
	} collision();

	iShowBMP2(c1X, c1Y, "image\\c1.bmp", 0);
	iShowBMP2(c2X, c2Y, "image\\c2.bmp", 0);
	iShowBMP2(c1X, c3Y, "image\\c3.bmp", 0);

	c1Y = c1Y - 7;
	if (c1Y <= 0){
		c1Y = 600;
		c1X = 400 + rand() % 350;
	}coinCollision();

	c2Y = c2Y - 7.5;
	if (c2Y <= 0){
		c2Y = 600;
		c2X = 360 + rand() % 300;
	}coinCollision();

	c3Y = c3Y - 8;
	if (c3Y <= 0){
		c3Y = 600;
		c3X = 310 + rand() % 350;
	}coinCollision();


	drawTimer();

}
void startButtonClickHandler()
{
	homePage = 0;
	startPage = 1;
	aboutPage = 0;
	instructionPage = 0;
}

void aboutButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 1;
	instructionPage = 0;
}

void instructionButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 1;
}
void scoreButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 0;
	scorePage = 1;
}
void backButtonClickHandler()
{
	homePage = 1;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 0;
	scorePage = 0;
}
void easyButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 0;
	scorePage = 0;
	easyPage = 1;
}
void mediumButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 0;
	scorePage = 0;
	easyPage = 0;
	mediumPage = 1;
}
void hardButtonClickHandler()
{
	homePage = 0;
	startPage = 0;
	aboutPage = 0;
	instructionPage = 0;
	scorePage = 0;
	easyPage = 0;
	mediumPage = 0;
	hardPage = 1;
}

void gameover()
{
	iSetColor(128, 128, 128);
	iFilledRectangle(0, 0, 1000, 600);
	iShowBMP2(0, 0, "image\\GameOver.bmp", 0);
	iShowBMP2(20, 475, "image\\Back2.bmp", 0);
	homePage = 2;  
}

int main()
{
	if (musicOn)
	PlaySound("CarMusicOn.wav", NULL, SND_LOOP | SND_ASYNC);
	inatializeImagePosition();
	iSetTimer(10, moveBackground);
	iSetTimer(1000, changeTimer);
	iInitialize(1000, 600, "Car Racing Game");
	iStart(); // Start the drawing loop
	return 0;
}
