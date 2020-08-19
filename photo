
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//struct to hold a single pixel
typedef struct pixel_struct {
	int red;
	int green;
	int blue;
} pixel;

//struct to hold an image made up of pixels
typedef struct image_struct {
	char p;
	int format;
	int columns;
	int rows;
	int value;
	pixel *rgb;
} image;

//struct to hold an array of multiple images
typedef struct picArray_struct { pixel **pictures; } picArray;

//Function to upload an image from a file
//arg *name is a string containing a filename
image uploadImage(char *name) {
  
	FILE *file;
	
	//initialize image pic
	image pic;
  
  //open file
	file = fopen(name, "r");
  
	if (file == NULL) {
		printf("Could not open %s\n", name);
	}
  
	else {
	  //read formatting info from file and add to pic
		fscanf(file, "%c%d\n", &pic.p, &pic.format);
		fscanf(file, "%d %d\n", &pic.columns, &pic.rows);
		fscanf(file, "%d\n", &pic.value);
	}
  

  //allocate memory for pixels
	pic.rgb = (pixel *)malloc((pic.rows * pic.columns) * sizeof(pixel));
  
  //read pixel data from file and add to pic.rgb
	for (int i = 0; i < (pic.rows * pic.columns); i++) {
		pixel test;
		fscanf(file, "%d %d %d", &test.red, &test.green, &test.blue);
		pic.rgb[i] = test;
	}

  //close file
	fclose(file);

  //print pic to console
	for (int i = 0; i < (pic.rows * pic.columns); i++) {
		pixel x = pic.rgb[i];
		
	}

	return pic;
}


// function to add pictures to the picArray
//args num_pics = number of images, pic = the array containing all images, *img, a pointer to a single image
void addToArray(int num_pics, picArray pic, image *img) {
	for (int i = 0; i < (num_pics + 1); i++) {
		pixel *ptr = pic.pictures[i];
		if (ptr == NULL) {
			pic.pictures[i] = img->rgb;
			break;
		}
	}
}

//function to create a file to output image into
//args length = total pixels, *end = pointer to final image
void createFile(int length, image *end) {
	FILE *fnew;
	char *name;
	
	printf(" \n");
	//asks user for file to output image to
	printf("Enter the name of a file to print to: ");
	scanf("%s", name);
	
	//opens file
	fnew = fopen(name, "w");
	
	//writes formatting data to file
	fprintf(fnew, "%c%d\n", end->p, end->format);
	fprintf(fnew, "%d ", end->columns);
	fprintf(fnew, "%d\n", end->rows);
	fprintf(fnew, "%d\n", end->value);
	
	//prints all pixels to file
	for (int i = 0; i < length; i++) {
		pixel w = end->rgb[i];
		fprintf(fnew, "%d %d %d\n", w.red, w.green, w.blue);
	}
	//close file
	fclose(fnew);
}

//Function to compare each image in the array
//This is where the average filter is applied
//args array = a picArray, num_pics = the number of images, length = number of pixels, *pic a pointer to an image
void comparison(picArray array, int num_pics, int length, image *pic) {
  
  //allocate space for pixels
	pic->rgb = (pixel *)malloc((length) * sizeof(pixel));

  //copy first image to pic
  pixel *ptr = array.pictures[1];
	 for (int i = 0; i < length; i++) {
		pic->rgb[i] = ptr[i];
	}

  //add the color values of each pixel in each image in the array together
	for (int i = 2; i < (num_pics + 1); i++) {
		pixel *ptr = array.pictures[i];
		for (int i = 0; i < length; i++) {
			pixel x = ptr[i];
			pixel y = pic->rgb[i];

			x.red += y.red;
			x.green += y.green;
			x.blue += y.blue;

			pic->rgb[i] = x;
		}
	}
	
  //set pixel values
	printf(" \n");
	for (int i = 0; i < length; i++) {
		pixel q = pic->rgb[i];
	}
  
  //divide each color value by number of images
	for (int i = 0; i < (length); i++) {
		pixel z = pic->rgb[i];
		pixel done;
		done.red = z.red / num_pics;
		done.green = z.green / num_pics;
		done.blue = z.blue / num_pics;

		pic->rgb[i] = done;
	}

  //call create file
	createFile(length, pic);
}

//main function
int main(void) {
  
  
	FILE *f;
	char *filename;
	char array[50][180];
	int count = 0;

  //get name of file containing list of image files
	printf("Enter a .txt file containing a list of .ppm files\n");
	scanf("%s", filename);
  
  //open file
	f = fopen(filename, "r");
	if (f == NULL) {
		printf("file could not be opened");
	}

	else {
		while (!feof(f)) {
		  //read each line and add each filename to an array of strings
			fscanf(f, "%s", array[count]);
			
			//keep track of number of images
			count++;
		}
	}
	fclose(f);

  //number of images
	int num_pics = count - 1;

  //initialize arrayOfPictures and allocate memory
	picArray arrayOfPictures;
	arrayOfPictures.pictures = (pixel **)malloc((count) * sizeof(pixel *));
	image p;
	
	//upload each image and add it to arrayOfPictures
	for (int i=0; i<num_pics; i++){
	  p = uploadImage(array[i]);
	  addToArray(num_pics, arrayOfPictures, &p);
	  
	}
	
	int length = p.rows * p.columns;
	
	//save the header
	char formatletter = p.p;
	int formatnumber = p.format;
	int base = p.columns;
	int height = p.rows;
	int max = p.value; 

  //initialize final image end, set formatting data, and allocate memory
	image end;
	end.p = formatletter;
	end.format = formatnumber;
	end.columns = base;
	end.rows = height;
	end.value = max;
	end.rgb = (pixel *)malloc((length) * sizeof(pixel));
	//call comparison to apply average filter to images and output final image to file
	comparison(arrayOfPictures, num_pics, length, &end);

	
}
