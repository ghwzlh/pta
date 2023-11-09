```c++
void merge_pass( ElementType list[] ,ElementType sorted[],int N,int length)
{
   
   for(int i = 0 ; i < N ; i = i + 2*length)
   {
      int num1 = i;
      int num2 = i+length;
      int num = i;
      int last ;
       if(i+2*length >= N )
           last = N;
       else
           last = i+2*length;
      while(num1 < i+length && num2 < last && num1 < N && num2 < N)
      {
            if(list[num1] < list[num2])
                sorted[num++] = list[num1++];
            else
                sorted[num++] = list[num2++];
      }
      while(num1 < i+length)
         sorted[num++] = list[num1++];
      while(num2 < last )
         sorted[num++] = list[num2++];
   }
}

```

