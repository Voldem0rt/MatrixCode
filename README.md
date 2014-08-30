#ifndef MATRIX_H
#define MATRIX_H

#include <iostream>
using namespace std;
class matr{
    int rows;
    int columns;
    float **fData;

    friend istream &operator>>(istream&, matr& );
    friend ostream &operator<<(ostream&, matr& );
public:
    matr(int = 1, int = 1);
    ~matr();
    void setMatrix(int, int, float);
    void fillMatrix();
    void getMatrix();

    matr operator-(const matr&)const;
    matr operator+(const matr&)const;
    matr operator*(const matr&)const;
};

#endif // MATRIX_H

#include <iostream>
#include "Matrix.h"
using namespace std;

int main()
{
    matr a(4, 4), b(4, 4);
    a.fillMatrix();
    a.getMatrix();
    b.fillMatrix();
    b.getMatrix();
    cout << endl;
    (a - b).getMatrix();
    cout << endl;
    (a + b).getMatrix();
    cout << endl;
    (a * b).getMatrix();
    //a.~matr();
    //b.~matr();
    return 0;
}


#include <iostream>
#include <Matrix.h>
using namespace std;

matr::matr(int x, int y){
    if( !x || !y){
        x = 1; y = 1;}
    if( x == 0 || y == 0 ){
        x = 1; y = 1;
    }
    rows = x;
    columns = y;
    fData = new float* [rows];
    for(int i = 0; i < rows; ++i)
        fData[i] = new float[columns];
    
}

matr::~matr(){
    for(int i = 0; i < rows; ++i)
        delete fData[i];
    delete []fData;
}

void matr::setMatrix(int x, int y, float data){
    if( x < 0 || x > rows )
        x = 0;
    if( y < 0 || y > columns )
        y = 0;
    fData[x][y] = data;
}

matr matr::operator+(const matr& rvalue)const{
    matr temp(*this);              // temporary matrix;
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
    temp.fData[i][j] = temp.fData[i][j] + rvalue.fData[i][j];
    }
    return temp;
}

matr matr::operator-(const matr& rvalue)const{
    matr temp(*this);
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            temp.fData[i][j] = temp.fData[i][j] - rvalue.fData[i][j];
    }
    return temp;
}

matr matr::operator*(const matr& rvalue)const{
    matr temp(*this);
    matr temp1(rows, columns);
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            temp1.fData[i][j] = 0;
    }
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j){
            for(int k = 0; k < columns; ++k)
                temp1.fData[i][j] = temp1.fData[i][j] + temp.fData[i][k] * rvalue.fData[k][j];
        }
    }
    return temp1;
}
void matr::fillMatrix(){
    cout << "Enter elements of matrix: " << endl;
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            cin >> fData[i][j];
        cout << endl;
    }

}

void matr::getMatrix(){
    cout << "Matrix is: " << endl;
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            cout << fData[i][j] << " ";
        cout << endl;
    }
}
/*
istream &operator>>(istream& stream_in, matr& rvalue){
    cout << "Enter elements of matrix: " << endl;
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            stream_in >> rvalue.fData[i][j];
        cout << endl;
    }
    return stream_in;
}

ostream &operator<<(ostream& stream_out, matr& rvalue ){
    cout << "Matrix is: " << endl;
    for(int i = 0; i < rows; ++i){
        for(int j = 0; j < columns; ++j)
            stream_out << rvalue.fData[i][j];
        cout << endl;
    }
    return stream_out;
}
*/


Как-то так у меня вышло.Причем я не понимаю,как перегрузить оператор вывода.Как есть,не работает.Не догоняю.
Еще не могу понять,почему она выдает только одно действие.Чтобы получить результат,нужно комментить два из трех действий.
