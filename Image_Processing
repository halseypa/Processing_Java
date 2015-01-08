//save the program and put the images in the created directory
//i.e., Documents>Processing>Image_Processing
PImage me = loadImage("_______.jpg");

size(me.width, me.height);
background(0);
//image(me, 0, 0);

//reconstruction controls
//noise spread; larger = more spread
int noiseMul = 60;
//determines centering. noiseMul/center: e.g., 10/2 results in +/-5 around noise value
int centerNoise = 2;

//jitter controls
//noise spread; larger = more spread
int pixelMul = 100;
//determines centering. pixelMul/center: e.g., 10/2 results in +/-5 around noise value
int pixelCenter = 2;
//number of pixels to be grabbed in the array
int pixelNum = int(random(1800));   

//number of iterations over the two processes
int globalRunNum = 4;
//number of pixel subsets grabbed from orignial image
int subsetGrabbed = 500;
//number of iterations to jitter sequences of pixels
int pixelJitterRunNum =0;

//largest height to select from
int heightMax = 750;
//largest width to select from
int widthMax = 750;


float[][] kernal = { { .11, .11, .11 }, { .11, .66, .11 }, { .11, .11, .11 } };


for (int currentRun = 0; currentRun < globalRunNum; currentRun++) {
  
  println(currentRun);
  
  for (int i = 0; i < subsetGrabbed; i++) {
  
    int xRand = int(random(me.width));
    int yRand = int(random(me.height));
   
    int randHeight = int(random(heightMax));
    int randWidth = int(random(widthMax));
  
    int xPerlinNoise = (int(noise(random(10))*noiseMul))-(noiseMul*centerNoise);
    int yPerlinNoise = (int(noise(random(10))*noiseMul))-(noiseMul*centerNoise);

    //filter(GRAY);
    tint(255, 20);
    
    //"watercolor" version
    copy(me, xRand, yRand, randWidth, randHeight, xRand, yRand, (randWidth+xPerlinNoise), (randHeight+yPerlinNoise));
    
    //glitch version
    //copy(me, xRand, yRand, randWidth, randHeight, (xRand+xPerlinNoise), (yRand+yPerlinNoise), randWidth, randHeight);
  }

  save("reconstruction.jpg");

  PImage recon = loadImage("reconstruction.jpg");

  recon.loadPixels();
  loadPixels();

  int count = recon.width*recon.height;

  for (int j = 0; j < pixelJitterRunNum; j++) {
  
    int randStart = int(random(count));
    int arrayJitter = int(noise(random(10))*pixelMul)-(pixelMul*pixelCenter);
  
    for (int i = 0; i < pixelNum; i++) {
    
      int pixelIncrement = (randStart+i)%count;
      int pixelJitterWrap = (randStart+i+arrayJitter)%count;
    
      if (pixelJitterWrap < 0) {
        pixelJitterWrap = pixelJitterWrap + count;
      }
    
      pixels[pixelIncrement] = pixels[pixelJitterWrap%count];
    }
  }
  updatePixels();
  
  save("jitterReconstruction.jpg");
  
  me = loadImage("jitterReconstruction.jpg");
  
}

PImage edgeImg = createImage(me.width, me.height, RGB);

int count = me.width*me.height;

for (int y = 0; y < me.height-1; y++) {
  for (int x = 0; x < me.width-1; x++) {
    
    float sum = 0;
    
    for(int ky = -1; ky <= 1; ky++) {
      for (int kx = -1; kx <= 1; kx++) {
        
        int pos = (y + ky)*width + (x +kx);
        
        if (pos < 0) {
          pos = pos+count;
        }
        
        float val = red(me.pixels[pos%count]);
        
        sum += kernal[ky+1][kx+1] * val;
      }
    }
    
    edgeImg.pixels[y*me.width +x] = color(sum);
  }
}
edgeImg.updatePixels();
image(edgeImg, 0, 0);

save("convolutionReconstruction.jpg");
