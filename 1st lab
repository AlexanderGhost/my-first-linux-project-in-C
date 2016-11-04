#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<linux/types.h>					//указывает компилятору, что нужно подставить строку, определенную аргументом token-string

#define MAX_LINE 80

int main(void) {
	char *args[MAX_LINE / 2 + 1];			// Массив для хранения аргументов коммандной строки
	char buffer[80];						// буфер для входа данных пользователя
	char *token = NULL;						// массив лексем
	char *whiteSpace = " \t\n\f\r\v";		// массив символов пробелов
	int c;									// переменная для хранения одного символа ввода - используется для детекта выхода из программы
	do {
		fflush(stdout);						// оставляем входной поток открытым (?)
		printf("input> ");					// ввод
		fflush(stdout);
		scanf(" %[^\n]", buffer);			// получем данные, введенные с клавы (?)

		int count = 0;						// переменная для хранения кол-ва параметров коммандной строки
		token = strtok(buffer, whiteSpace);
		args[count] = strdup(token);		// получаем первую строку с помощью strdup

		count += 1;							//Циклично считываем вводимую строку(?) посимвольно(?!)
		while ((token = strtok(NULL, whiteSpace)) != NULL) {
			args[count] = strdup(token);
			count += 1;
		}
		args[count] = NULL;					// завершаем формированную строку, которую должны передать функции execvp нулем

		pid_t pid = fork();					// создаем процесс
		if (pid == 0) {						// если успешно, то
			if ((execvp(args[0], args)) == -1) {	// мы в дочерном процессе, тут вызываем execvp и проверяем успешность(т.е обращаем внимание на строку 30)
				printf("Error\n");
				exit(1);
			}
		} else if (pid > 0) {				// а тут мы в родительском процессе
			if (args[count - 1] == "&")		// ожидаем
				wait();
			else
				waitpid(pid, 0, 0);
		} else {
			exit(1);
		}

	}while((c=getchar()) != EOF);			// завершаем программы если введен символ EOF
	return 0;
}
