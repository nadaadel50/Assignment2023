// CS213-2023-20221179-20221042-20220130-A1-Part1.cpp

// FCAI - OOP Programming - 2023 - Assignment 1
// Program Name: CS213-2023-20221179-20221042-20220130-A1-Part1.cpp
// Last Modification Date : 10/10/2023
// Author1 and ID : Nada Adel Ahmed Nagy          (20221179) Email: dew.sara2004@gmail.com
// Author2 and ID : Touka Atef El-Sayed Abu-ElAzm (20221042) Email: toukaatef48@gmail.com
// Author3 and ID : Rana Ibrahim Bassiouny        (20220130) Email: roniibassiouny@gmail.com
// Teaching Assistant:
// Purpose: To learn how to deal with gray images using C++

#include <iostream>
#include <fstream>
#include <cstring>
#include <cmath>
#include "bmplib.cpp"

using namespace std;
// Creating 2D arrays
unsigned char image[SIZE][SIZE];
unsigned char image2[SIZE][SIZE];
unsigned char image_f[SIZE][SIZE];
// To enter the number of the filter you want to use
char choice;
int degree;
int dx[] = {-1, 1, 0, 0 , -1, -1, 1, 1};
int dy[] = {0, 0, -1, 1, -1, 1, -1, 1};
void loadImage ();
void saveImage ();
void doSomethingForImage ();

int main()
{
    loadImage();
    doSomethingForImage();
    while(choice != '0'){
        // Selecting the filter
        cout << "Please, select a filter to apply " << endl;
        cout << "1- Black & White Filter" << endl;
        cout << "2- Invert Filter" << endl;
        cout << "3- Merge Filter" << endl;
        cout << "4- Flip Filter" << endl;
        cout << "5- Rotate Filter" << endl;
        cout << "6- Darken & Lighten Filter" << endl;
        cout << "7- Detect Image Edges" << endl;
        cout << "8- Enlarge Image" << endl;
        cout << "9- Shrink Image" << endl;
        cout << "a- Mirror 1/2 Image" << endl;
        cout << "b- Shuffle Image" << endl;
        cout << "c- Blur Image" << endl;
        cout << "d- Crop Image" << endl;
        cout << "e- skew Image Right" << endl;
        cout << "f- skew Image Up" << endl;
        cin >> choice;
        if(choice == '3'){
            char image2File[100];
            // To take the second image to merge it with the first picture
            cout << "Enter the source image file name of the second image:";
            cin >> image2File;
            // Add to it .bmp extension and load image
            strcat (image2File, ".bmp");
            readGSBMP(image2File, image2);
        }
        if(choice != 0){
            doSomethingForImage();
        }
    }
    saveImage();

    return 0;
}

//_________________________________________
void loadImage () {
    char imageFileName[100];
    // Get gray scale image file name
    cout << "Enter the source image file name: ";
    cin >> imageFileName;
    // Selecting the filter
    cout << "Please, select a filter to apply " << endl;
    cout << "1- Black & White Filter" << endl;
    cout << "2- Invert Filter" << endl;
    cout << "3- Merge Filter" << endl;
    cout << "4- Flip Filter" << endl;
    cout << "5- Rotate Filter" << endl;
    cout << "6- Darken & Lighten Filter" << endl;
    cout << "7- Detect Image Edges" << endl;
    cout << "8- Enlarge Image" << endl;
    cout << "9- Shrink Image" << endl;
    cout << "a- Mirror 1/2 Image" << endl;
    cout << "b- Shuffle Image" << endl;
    cout << "c- Blur Image" << endl;
    cout << "d- Crop Image" << endl;
    cout << "e- skew Image Right" << endl;
    cout << "f- skew Image Up" << endl;
    cin >> choice;
    // Add to it .bmp extension and load image
    strcat (imageFileName, ".bmp");
    readGSBMP(imageFileName, image);
    if(choice == '3'){
        char image2File[100];
        // To take the second image to merge it with the first picture
        cout << "Enter the source image file name of the second image:";
        cin >> image2File;
        // Add to it .bmp extension and load image
        strcat (image2File, ".bmp");
        readGSBMP(image2File, image2);
    }
}

//_________________________________________
void saveImage () {
    char imageFileName[100];
    // Get gray scale image target file name
    cout << "Enter the target image file name:";
    cin >> imageFileName;
    // Add to it .bmp extension and load image
    strcat (imageFileName, ".bmp");
    writeGSBMP(imageFileName, image_f);

}

//_________________________________________
void doSomethingForImage() {
    if(choice == '1'){
        // Black and white image
        // We need to change every pixel that is greater than 127 to 255 to make it white and if it is
        // less than 127 we change it to 0 to make it black
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j< SIZE; j++) {
                if (image[i][j] > 127){
                    image_f[i][j] = 255;
                    image[i][j] = image_f[i][j];
                }
                else {
                    image_f[i][j] = 0;
                    image[i][j] = image_f[i][j];
                }
            }
        }
    }
    else if(choice == '2'){
        // Invert image
        for(int i = 0; i < SIZE; i++){
            for(int j = 0; j < SIZE; j++){
                image_f[i][j] = 255 - image[i][j];
                image[i][j] = image_f[i][j];
            }
        }
    }
    else if(choice == '3'){
        // Merge images
        // We need to get the average to get the merged picture from image1 and image2
        for(int i = 0; i < SIZE; i++){
            for(int j = 0; j < SIZE; j++){
                image_f[i][j] = (image[i][j] + image2[i][j])/2;
                image[i][j] = image_f[i][j];
            }
        }
    }
    else if(choice == '4'){
        // Flip images
        // We need to swap pixels to flip the image by using this algorithm
        cout << "Flip (h)orizontally or (v)ertically? " << endl;
        char direction; cin >> direction;
        if(direction == 'H' || direction == 'h'){
            // We figure out from drawing a matrix that (i) doesn't change but (j) changes with this algorithm
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE/2+1; j++){
                    int tmp = image[i][j];
                    image_f[i][j] = image[i][SIZE-1-j];
                    image_f[i][SIZE-1-j] = tmp;
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(direction == 'V' || direction == 'v'){
            // We figure out from drawing a matrix that (j) doesn't change but (i) changes with this algorithm
            for(int i = 0; i < SIZE/2+1; i++) {
                for (int j = 0; j < SIZE; j++) {
                    int tmp = image[i][j];
                    image_f[i][j] = image[SIZE-1-i][j];
                    image_f[SIZE-1-i][j] = tmp;
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
        }
    }
    else if(choice == '5'){
        cout << "Rotate (90), (180) or (270) degrees: ";
        cin >> degree;
        if(degree == 90){
            // By drawing a matrix and rotate it with 90 degrees we figure out that we need to use a new 2D array (image2)
            // Looping on each pixel and put them in the right place by using this algorithm
            for(int i = 0; i <SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image_f[j][SIZE-1-i] = image[i][j];
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(degree == 180){
            // To make the image rotate by 180 degree all we need is only to flip it vertical then horizontal
            // To flip it vertical
            for(int i = 0; i < SIZE/2+1; i++) {
                for (int j = 0; j < SIZE; j++) {
                    int tmp = image[i][j];
                    image_f[i][j] = image[SIZE-1-i][j];
                    image_f[SIZE-1-i][j] = tmp;
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
            // To flip it horizontal
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE/2+1; j++){
                    int tmp = image[i][j];
                    image_f[i][j] = image[i][SIZE-1-j];
                    image_f[i][SIZE-1-j] = tmp;
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(degree == 270){
            // By drawing a matrix and rotate it with 270 degrees we figure out that we need to use a new 2D array (image2)
            // Looping on each pixel and put them in the right place by using this algorithm
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image_f[SIZE-1-j][i] = image[i][j];
                }
            }
            // To put the new image after using the filter in the variable image
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] = image_f[i][j];
                }
            }
        }
    }
    else if(choice == '6'){
        cout << "Do you want to (d)arken or (l)ighten? ";
        char dl; cin >> dl;
        // Darken the image by 50% all we need is to divide each pixel by two
        if (dl == 'd' || dl == 'D'){
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] /= 2;
                    image_f[i][j] = image[i][j];
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(dl == 'l' || dl == 'L'){
            // To lighten the image we can't to multiply each pixel by 1.5 cause some pixels might by more than 255 or some pixels might equal zero
            // We can add 50% of the difference between (255) and image[i][j] , so we get rid of the problem
            for(int i = 0; i < SIZE; i++){
                for(int j = 0; j < SIZE; j++){
                    image[i][j] += (255-image[i][j])/2;
                    image_f[i][j] = image[i][j];
                    image[i][j] = image_f[i][j];
                }
            }
        }
    }
    else if(choice == '9'){
        // Shrink image
        cout << "Shrink to (1/2), (1/3) or (1/4)? " << endl;
        string shrink; cin >> shrink;
        // To make the background white we will loop in every pixel and make it equal 255
        for(int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                image_f[i][j] = 255;
            }
        }
        if(shrink == "1/2"){
            // We will need to make the image of size 64*64 , so we need to put each 2 pixels in 1 pixel only in image2
            for(int i = 0; i < SIZE/2+1; i++){
                for(int j = 0; j < SIZE/2+1; j++){
                    image_f[i][j] = image[i*2][j*2];
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(shrink == "1/3"){
            // We will need to make the image of size 32*32, so we need to put each 3 pixels in 1 pixel only in image2
            for(int i = 0; i < SIZE/3+1; i++){
                for(int j = 0; j < SIZE/3+1; j++){
                    image_f[i][j] = image[i*3][j*3];
                    image[i][j] = image_f[i][j];
                }
            }
        }
        else if(shrink == "1/4")
            // We will need to make the image of size 16*16, so we need to put each 4 pixels in 1 pixel only in image2
            for(int i = 0; i < SIZE/4+1; i++){
                for(int j = 0; j< SIZE/4+1; j++){
                    image_f[i][j] = image[i*4][j*4];
                    image[i][j] = image_f[i][j];
                }
            }
    }
    else if(choice == 'c'){
        // Blur the image
        // We will use a directional array to calculate the average of the neighbours pixels
        for(int i = 0; i < SIZE; i++){
            for(int j = 0; j < SIZE; j++){
                int sum = 0;
                for(int r = 0; r < 8; r++){
                    int next_x, next_y;
                    next_x = i + dx[r];
                    next_y = j + dy[r];
                    sum += image[next_x][next_y];
                }
                image_f[i][j] = sum / 8;
            }
        }
        // To put the new image after using the filter in the variable image
        for(int i = 0; i < SIZE; i++){
            for(int j = 0; j < SIZE; j++){
                image[i][j] = image_f[i][j];
            }
        }
    }
}


