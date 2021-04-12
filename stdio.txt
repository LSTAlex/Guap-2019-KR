#include <iostream>
#include <fstream>
#include<windows.h>

using namespace std;

ofstream fout;
ifstream fin;

bool is_znak(char c)
{
	if (c == '.' || c == '?' || c == '!')
		return true;
	return false;
}

bool is_space(char c)
{
	if (c == ' ' || c == '\t' || c == '\n')
		return true;
	return false;
}
// Считываем предложение до знака .?! или до конца файла
void readSentence(FILE* fin, char* s)
{
	int i = 0;
	while (1)
	{
		s[i] = fgetc(fin);
		if (is_znak(s[i]))
			break;
		if (feof(fin))
		{
			s[i] = '\0';
			break;
		}
		i++;	
	}
	s[i + 1] = '\0';
}

// s1, s2, s3 - три предложения,
// r1, r2 - разделители между ними (группы пробельных символов)a
void ReadFile(char* s1, char* r1, char* s2, char* r2, char* s3)
{
	const char* name = "text.txt";
	FILE* fin = fopen(name, "r");
	if (!fin)
	{
		cout << "Ошибка открытия файла!" << endl;
		return;
	}
	readSentence(fin,s1);
	int i = 0;
	char c = fgetc(fin);
	while (is_space(c)) // пока пробельный символ
	{
		r1[i++] = c; // заносим в строку разделитель
		c = fgetc(fin);
	}
	r1[i] = '\0';
	s2[0] = c;

	readSentence(fin, s2 + 1);
	i = 0;
	c = fgetc(fin);
	while (is_space(c)) // пока пробельный символ
	{
		r2[i++] = c; // заносим в строку разделитель
		c = fgetc(fin);
	}
	r2[i] = '\0';
	s3[0] = c;
	readSentence(fin, s3 + 1);
	fclose(fin);
}

void Output(char* s1, char* r1, char* s2, char* r2, char* s3)
{
	FILE* fout = fopen("newtext.txt", "w");
	fprintf(fout, "%s", s3);
	if (*r1)
	fprintf(fout, "%s", r1);
	fprintf(fout, "%s", s2);
	if (*r2)
	fprintf(fout, "%s", r2);
	fprintf(fout, "%s\n", s1);
	fclose(fout);	
}


int main()
{
	int MAX_SIZE = 300;
	char* s1 = new char[MAX_SIZE];
	char* s2 = new char[MAX_SIZE];
	char* s3 = new char[MAX_SIZE];
	char* r1 = new char[MAX_SIZE];
	char* r2 = new char[MAX_SIZE];
	ReadFile(s1, r1, s2, r2, s3);
	Output(s1, r1, s2, r2, s3);
	delete[] s1;
	delete[] s2;
	delete[] s3;
	delete[] r1;
	delete[] r2;
	_CrtSetReportMode(_CRT_WARN, _CRTDBG_MODE_FILE);
	_CrtSetReportFile(_CRT_WARN, _CRTDBG_FILE_STDOUT);
	_CrtSetReportMode(_CRT_ERROR, _CRTDBG_MODE_FILE);
	_CrtSetReportFile(_CRT_ERROR, _CRTDBG_FILE_STDOUT);
	_CrtSetReportMode(_CRT_ASSERT, _CRTDBG_MODE_FILE);
	_CrtSetReportFile(_CRT_ASSERT, _CRTDBG_FILE_STDOUT);
	_CrtDumpMemoryLeaks();
	system("pause");
	return 0;
}