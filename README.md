    /**
     * recover.c
     *
     * Computer Science 50
     * Problem Set 4
     *
     * Recovers JPEGs from a forensic image.
     */

     #include <stdio.h>
     #include <stdint.h>

    typedef uint8_t BYTE;

    int main(void)
    {
        FILE* file = fopen("card.raw","r"); 



        if(file == NULL)
        {

            fclose(file);
            printf("Unable to open the file\n");
            return 1;
        }


        int  file_no = 0;
        char filename [10];
        BYTE buffer[512];
        FILE* ptr = NULL;

        while(fread(buffer, 512, 1, file) != 0)
        {
           if(buffer[0] == 0xff && buffer[1] == 0xd8  && buffer[2] == 0xff && (buffer[3] == 0xe0|| buffer[3] == 0xff))
           {
              if(ptr != NULL)
              {
                  fclose(ptr);
              }

            sprintf(filename, "%03d.jpg", file_no);
            ptr = fopen(filename,"w");
            fwrite(buffer, 512, 1, ptr);

            file_no++;

        }
        else if (file_no > 0)
        {
            fwrite(buffer, 512, 1, ptr);
        }



        }
        fclose(file);
        return 0;
    }
