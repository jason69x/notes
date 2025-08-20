```c
#include<stdio.h>

#include<math.h>

  

int calc_zcr(int*,int );

double calc_energy(int*,int );

double euclidean(double*,double*);

  

int main(){

  int yes_smpl[51329];

  int no_smpl[6440];

  int input_smpl[51329];

  int y=0,n=0,in=0;

  FILE *yesf = fopen("yes.txt","r");

  while(fscanf(yesf, "%d",&yes_smpl[y])==1){

    y++;

  }

  FILE *nof = fopen("no.txt","r");

  while(fscanf(nof, "%d",&no_smpl[n])==1){

    n++;

  }

  FILE *inputf = fopen("input.txt","r");

  while(fscanf(inputf, "%d",&input_smpl[in])==1){

    in++;

  }

  fclose(yesf);

  fclose(nof);

  fclose(inputf);

  double yes_nrg,no_nrg,input_nrg;

  int yes_zcr,no_zcr,input_zcr;

  yes_nrg = calc_energy(yes_smpl,y);

  yes_zcr = calc_zcr(yes_smpl,y);

  no_nrg = calc_energy(no_smpl,n);

  no_zcr = calc_zcr(no_smpl,n);

  input_nrg = calc_energy(input_smpl,in);

  input_zcr = calc_zcr(input_smpl,in);

  double yes_stats[] = {yes_nrg,yes_zcr};

  double no_stats[] = {no_nrg,no_zcr};

  double input_stats[] = {input_nrg,input_zcr};

yes_stats[0] /= 1e6; // scale energy down

no_stats[0]  /= 1e6;

input_stats[0] /= 1e6;

  double yes_dist = euclidean(yes_stats,input_stats);

  double no_dist = euclidean(no_stats,input_stats);

  

  if(yes_dist<no_dist){

    printf("YES");

  }

  else{

    printf("NO");

  }

}

  

double calc_energy(int *sample,int n){

  double energy = 0;

  for(int i=0;i<n;i++){

    energy += *(sample+i) * *(sample+i);

  }

  energy /= n;

  return energy;

}

  

int calc_zcr(int *sample,int n){

  int zcr = 0;

  for(int i=1;i<n;i++){

    if((*(sample+i-1)>0 && *(sample+i) < 0) || (*(sample+i-1)<0 && *(sample+i)>0)){

      zcr++;

    }

  }

  return zcr;

}

  

double euclidean(double *yes_stats,double *no_stats){

  double sum = 0.0;

  for(int i=0;i<2;i++){

    double diff = yes_stats[i] - no_stats[i];

    sum += diff * diff;

  }

  return sqrt(sum);

}
```

