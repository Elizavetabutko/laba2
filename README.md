## Отчет по лабораторной работе № 2

#### № группы: `ПМ-2601`

#### Выполнил: `Бутко Елизавета`

#### Вариант: `1`

### Условие
Напишите программу на Java, которая выполняет следующие действия
с двумерным массивом целых чисел:
1. Считывает с консоли размеры массива N и M, а затем сам массив
размером N × M.
2. Переставляет строки массива в порядке возрастания суммы их элементов. Если суммы равны, то сравнивает медианы строк. Если
медианы равны, сортирует строки по возрастанию дисперсии элементов. Если дисперсии равны, сохраняет исходный порядок.
3. Находит и выводит максимальное значение среди минимальных
элементов каждого столбца, а также номер столбца, в котором этот
минимум был найден.
4. Выводит элементы массива в виде спирали, начиная с левого верхнего элемента и двигаясь по часовой стрелке. Реализовать алгоритм
с помощью одного цикла.
5. Поворачивает массив на 90◦ по часовой стрелке и выводит его.
### Программа

```java
import java.io.PrintStream;
import java.util.Scanner;
public class Main {
    public static Scanner in = new Scanner(System.in);
    public static PrintStream out = System.out;
    public static void main(String[] args) {
        int n = in.nextInt(); // количество строк
        int m = in.nextInt(); // количество столбцов
        int[][] a = new int [n][m]; 
        for (int i = 0; i < n; i++){ // заполняем массив значениями
          for (int j = 0; j < m; j++){
            a[i][j] = in.nextInt();
          }
        }
        out.println("Исходный массив"); // выводим исходный массив
        for (int i = 0; i<n; i++){
          for (int j = 0; j<m; j++){
            out.print(a[i][j]+" ");
          }
          out.println();
        }
        int[] ss = new int [n]; // сумма элементов каждой строки
        for (int i = 0; i<n; i++){ 
          for (int j = 0; j<m; j++){
            ss[i] = ss[i] + a[i][j];
          }
        }
        double[] mm = new double [n]; // медиана каждой строки
        int[]cc = new int [m]; // создаем копию массива
        for (int i = 0; i<n; i++){
          for (int j = 0; j<m; j++){
            cc[j] = a[i][j];
          }
          for (int j = 0; j<m-1; j++) { // сортируем элементы в строке
            for (int k = 0; k<m-1-j; k++) {
              if (cc[k] > cc[k + 1]) {
                int e = cc[k];
                cc[k] = cc[k + 1];
                cc[k + 1] = e;
              }
            }
          }
          if (m % 2 != 0){
            mm[i] = cc[m/2];
          } 
          else {
            mm[i] = (cc[m/2] + cc[m/2-1])/2.0;
          }
        }
        double[] dd = new double [n]; // дисперсия каждой строки
        for (int i = 0; i<n; i++){ 
          double sr = (double)ss[i]/m; // среднее арифметическое строки
          for (int j = 0; j<m; j++){
            dd[i] = dd[i]+(a[i][j]-sr)*(a[i][j]-sr);
          }
          dd[i] = dd[i]/m;
        }
        int[][]aa = new int [n][m]; // создаем копию массива
        for (int i = 0; i<n; i++){
          for (int j = 0; j<m; j++){
            aa[i][j] = a[i][j];
          }
        }
        for (int i = 0; i<n-1; i++){ // сортируем строки массива пузырьком
          for (int j = 0; j<n-1-i; j++){
            boolean x = false;
            if (ss[j]>ss[j+1]){
              x = true;
            }
            else if (ss[j]==ss[j+1]){
              if (mm[j]>mm[j+1]){
                x = true;
              }
              else if (mm[j]==mm[j+1]){
                if (dd[j]>dd[j+1]){
                  x = true;
                }
              }
            }
            if (x){
              int[] y = a[j];
              a[j] = a[j+1];
              a[j+1] = y;
              int z = ss[j];
              ss[j] = ss[j+1];
              ss[j+1] = z;
              double k = mm[j];
              mm[j] = mm[j+1];
              mm[j+1] = k;
              double w = dd[j];
              dd[j] = dd[j+1];
              dd[j+1] = w;
            }
          }
        }
        out.println("Полученный массив"); // выводим полученный массив
        for (int i = 0; i<n; i++){
          for (int j = 0; j<m; j++){
            out.print(a[i][j]+" ");
          }
          out.println();
        }
        int[] mins = new int [m]; // минимальный элемент в столбце
        for (int j = 0; j<m; j++){
          int min = aa[0][j];
          for (int i = 1; i<n; i++){
            if (aa[i][j]<min){
              min = aa[i][j];
            }
          }
          mins[j] = min;
        }
        int max = mins[0];
        int q = 0;
        for (int j = 0; j<m; j++){ // находим максимальный из минимальных
          if (mins[j]>max){
            max = mins[j];
            q = j;
          }
        }
        out.print("Максимальный элемент из всех минимальных элементов: ");
        out.print(max);
        out.print(", находится в столбце: ");
        out.println(q+1);
        out.print("Вывод массива спиралью: ");
        int t = n*m; // общее количество элементов массива
        int up = 0; // вверхняя граница
        int down = n - 1; // нижняя граница
        int left = 0; // левая граница
        int right = m - 1; // правая граница
        int d = 0; // направления: 0 - двигаемся вправо, 1 - двигаемся вниз,     2 - двигаемся влево, 3 - двигаемся вверх
        int r = 0; // строка
        int c = 0; // столбец
        for (int i = 0; i < t; i++) {
          out.print(aa[r][c] + " ");
          if (d == 0) { 
            if (c == right) {
              up++;
              d = 1;
              r++;
            }
            else {
              c++;
            }
          }
          else if (d == 1) { 
            if (r == down) {
              right--;
              d = 2;
              c--;
            }
            else {
              r++;
            }
          }
          else if (d == 2) {
            if (c == left) {
              down--;
              d = 3;
              r--;
            }
            else {
              c--;
            }
          }
          else {
            if (r == up) {
              left++;
              d = 0;
              c++;
            }
            else {
              r--;
            }
          }
        }
        out.println();
        out.println("Поворачиваем массив на 90 градусов по часовой стрелке:");
        for (int i = 0; i<m; i++){ //поворачиваем массив на 90 градусов
          for (int j = n-1; j>=0; j--){
            out.print(aa[j][i]+" ");
          }
          out.println();
        }
    }
}
```
