## Отчет о лабороторной работе № 3

#### № группы: `ПМ-2502`

#### Выполнил: `Шаврин Радимир Вячеславович`

#### Вариант: `21`

### Cодержание:

- [Постановка задачи](#1-постановка-задачи)
- [Программа](#2-программа)
- [Анализ правильности решения](#3-анализ-правильности-решения)

### 1. Постановка задачи

> 
1. Вывод графа
Отображает все рёбра графа в порядке их добавления. Каждое ребро представле
но в формате: «вершина1- вершина2 (вес)». Вершины в каждом ребре упорядо
чены по возрастанию номеров.
2. Добавление ребра
Добавляет новое ребро с указанным весом между двумя вершинами. Рёбра между
одной парой вершин могут повторяться, но рёбра, соединяющие вершину саму с
собой, не добавляются.
3. Вывод вершин по убыванию номера
Отображает список всех вершин графа в порядке убывания их номеров.
4. Вершины с ограничением по весу входящих рёбер
Выводит вершины, укоторых общийвесвходящихрёбер не превышает указанного
значения.
5. Сортировка вершин по весу входящих рёбер
Выводит вершины в порядке убывания общего веса входящих рёбер. Если у двух
вершин одинаковый вес, они упорядочиваются по убыванию номеров.
6. Объединение рёбер с одинаковыми вершинами
Объединяет все рёбра между одной парой вершин в одно ребро с суммарным
весом. Новое ребро занимает место первого из объединяемых рёбер в порядке
добавления.
7. Объединение первого из рёбер с одинаковыми вершинами
Изменяет только первое из рёбер между одной парой вершин, добавляя к его весу
веса всех остальных рёбер между этими вершинами. Остальные рёбра остаются
неизменными.
8. Изменение веса рёбер
Изменяет вес всех рёбер между указанными вершинами на заданное значение (+
или −).
9. Удаление рёбер
Удаляет все рёбра между указанными вершинами. Порядок оставшихся рёбер в
графе остаётся неизменным.
35
10. Удаление вершины
Удаляет вершину из графа вместе со всеми рёбрами, в которых она участвует.
11. Удаление вершин с максимальным весом входящих рёбер и минималь
ным количеством входящих рёбер
Удаляет вершины, у которых общий вес входящих рёбер максимален, а также
вершины с минимальным количеством входящих рёбер.
12. Сложение двух графов
Объединяет два графа. Результирующий граф содержит все вершины и рёбра из
обоих графов, без повторений. Если между двумя вершинами появляются одина
ковые рёбра, их веса складываются.

### 2. Алгоритм

#### Алгоритм выполнения программы:


### 2. Программа

```java
import java.io.PrintStream;
import java.util.Scanner;
import java.io.IOException;

public class Main {
    public static Scanner in = new Scanner(System.in);
    public static PrintStream out = System.out;

    public static void main(String[] args) throws IOException {

        class Graph{

            int[] v1 = new int[100000];
            int[] v2 = new int[100000];
            double[] weights = new double[100000];
            int count = 0;

            public void outGraph(){
                for (int i = 0; i < count; i++){
                    out.println(Math.min(v1[i], v2[i]) + " - " + Math.max(v1[i], v2[i]) + "(" + weights[i] + ")");
                }
            }

            public void addGraph(int v_1, int v_2, double weight){
                if (v_1 != v_2){
                    v1[count] = Math.min(v_1, v_2);
                    v2[count] = Math.max(v_1, v_2);
                    weights[count] = weight;
                    count ++;

                }

            }

            public void outPeaks(){
                int[] mass = new int[100000];
                for (int i = 0; i < count; i++){
                    mass[v1[i]] +=1;
                    mass[v2[i]] +=1;

                }

                for(int i = mass.length-1; i > 0; i--){
                    if(mass[i] != 0){
                        out.print(i + " ");
                    }
                }
            }

            public void outNorm(double max){

                double[] mass = new double[100000];
                for (int i = 0; i < count; i++){
                    mass[v1[i]] += weights[i];
                    mass[v2[i]] += weights[i];
                }

                for(int i = 0; i < mass.length; i ++){
                    if(mass[i] <= max){
                        out.print(i + " ");
                    }
                }

            }

            public void outSort() {
                double[] mass = new double[100000];

                for (int i = 0; i < count; i++) {
                    mass[v1[i]] += weights[i];
                    mass[v2[i]] += weights[i];
                }

                int[] peaks = new int[100000];
                int count_peak = 0;

                for (int i = 0; i < mass.length; i++) {
                    if (mass[i] > 0) {
                        peaks[count_peak] = i;
                        count_peak++;
                    }
                }

                for (int i = 0; i < count_peak; i++) {
                    for (int j = 0; j < count_peak - 1; j++) {

                        int v_1 = peaks[j];
                        int v_2 = peaks[j + 1];
                        double w_1 = mass[v_1];
                        double w_2 = mass[v_2];

                        if (w_1 < w_2) {
                            int x = v_1;
                            v_1 = v_2;
                            v_2 = x;
                        }
                        else {
                            if (w_1 == w_2) {
                                if (v_1 < v_2) {
                                    int x = peaks[j];
                                    peaks[j] = peaks[j + 1];
                                    peaks[j + 1] = x;
                                }
                            }
                        }


                    }
                }

                for (int i = 0; i < count_peak; i++) {

                    out.print(peaks[i] + " ");
                }
            }

            public void unite(){

                int[] v1_2 = new int[100000];
                int[] v2_2 = new int[100000];
                double[] weights_2 = new double[100000];
                int count_2 = 0;

                for (int i = 0; i < count; i ++){
                    boolean flag = false;

                    for(int j = 0; j < count_2; j++){
                        if((v1[j] == v1_2[i]) && (v2[j] == v2_2[i])){
                            weights_2[j] += weights[j];
                            flag = true;
                        }

                    }

                    if(flag == false){
                        v1_2[count_2] = v1[i];
                        v2_2[count_2] = v2[i];
                        weights_2[count_2] = weights[i];
                        count_2++;
                    }

                }
                for(int i = 0; i < count_2; i ++){
                    v1[i] = v1_2[i];
                    v2[i] = v2_2[i];
                    weights[i] = weights_2[i];
                }
                for(int i = count_2; i < count; i ++){
                    v1[i] = 0;
                    v2[i] = 0;
                    weights[i] = 0;
                }
                count = count_2;


            }



            public void uniteFirst() {
                for (int i = 0; i < count; i++) {
                    boolean flag = true;

                    for (int j = 0; j < i; j++) {
                        if ((v1[i] == v1[j]) && (v2[i] == v2[j])) {
                            flag = false;
                        }
                    }

                    if (flag) {
                        double sum = weights[i];
                        for (int j = i + 1; j < count; j++) {
                            if ((v1[i] == v1[j]) && (v2[i] == v2[j])) {
                                sum += weights[j];
                            }
                        }

                        weights[i] = sum;
                    }
                }
            }

            public void change(int v1_2,  int v2_2, double x){

                for(int i = 0; i < count;i++){
                    if(v1[i] == Math.min(v1_2, v2_2) && v2[i] == Math.max(v1_2, v2_2)){
                        weights[i] +=x;
                    }
                }
            }

            public void delete(int v1_2,  int v2_2){
                int count_2 =0;
                for(int i = 0; i < count;i++){
                    if(v1[i] != Math.min(v1_2, v2_2) && v2[i] != Math.max(v1_2, v2_2)){
                        v1[count_2] = v1[i];
                        v2[count_2] = v2[i];
                        weights[count_2] = weights[i];
                        count_2++;
                    }
                }
                count = count_2;
            }

            public void deletev(int v){

                int count_2 =0;
                for(int i = 0; i < count;i++){
                    if(v1[i] != v && v2[i] != v){
                        v1[count_2] = v1[i];
                        v2[count_2] = v2[i];
                        weights[count_2] = weights[i];
                        count_2++;
                    }
                }
                count = count_2;
            }


            public void deleteusl(){
                double max = Math.pow(9,99) * (-1);
                double min = Math.pow(9,99);

                double weights_2[] = new double[100000];
                int[] counts = new int[100000];

                for(int i = 0; i < count; i++){
                    weights_2[v1[i]] += weights[i];
                    weights_2[v2[i]] += weights[i];
                    counts[v1[i]] ++;
                    counts[v2[i]] ++;

                }

                for(int i = 0; i < counts.length; i++){
                    if(weights_2[i] > max){
                        max = weights_2[i];
                    }
                    if(counts[i] < min){
                        min = counts[i];
                    }
                }

                for(int i = 0; i < counts.length; i ++){
                    if(weights_2[i] == max || counts[i] == min){

                        int count_2 =0;

                        for(int j = 0; j < count;j++){
                            if(v1[j] != i && v2[j] != i){
                                v1[count_2] = v1[j];
                                v2[count_2] = v2[j];
                                weights[count_2] = weights[j];
                                count_2++;
                            }
                        }
                        count = count_2;
                    }
                }

            }

            public Graph sum(Graph g2){

                Graph sum = new Graph();

                for(int i = 0; i < this.count; i++){
                    sum.v1[i] = this.v1[i];
                    sum.v2[i] = this.v2[i];
                    sum.weights[i] = this.weights[i];
                }
                sum.count = this.count;

                for(int i = 0; i < g2.count; i++){
                    boolean flag = false;

                    for (int j = 0; j < sum.count; j ++){
                        if((sum.v1[j] == g2.v1[i]) && (sum.v2[j] == g2.v2[i])){
                            sum.weights[j] += g2.weights[i];
                            flag = true;

                        }

                    }

                    if (flag == false){
                        sum.v1[sum.count] = g2.v1[i];
                        sum.v2[sum.count] = g2.v2[i];
                        sum.weights[sum.count] = g2.weights[i];
                        sum.count ++;
                    }
                }
                return sum;

            }



        }


    }



}



```

### 3. Анализ правильности решения

