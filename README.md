# pear_logo.h
The PearLogo pixel art in a 25x25px c header format

## How to include and use this file:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.>
#include <unistd.h>
#include "pear_logo.h"  //put the pear_logo.h in the same location as the C source is

int main(int argc, char * argv[]) {
        int j = 0;
        for (unsigned int y = 0; y <    gimp_image.height; y += 2) {
                for (unsigned int x = 0; x < gimp_image.width; x += 1) {
                        unsigned char r_t = gimp_image.pixel_data[(x + y * gimp_image.width) * 4];
                        unsigned char g_t = gimp_image.pixel_data[(x + y * gimp_image.width) * 4 + 1];
                        unsigned char b_t = gimp_image.pixel_data[(x + y * gimp_image.width) * 4 + 2];
                        unsigned char a_t = gimp_image.pixel_data[(x + y * gimp_image.width) * 4 + 3];
                        unsigned char r_b = 0;
                        unsigned char g_b = 0;
                        unsigned char b_b = 0;
                        unsigned char a_b = 0;
                        if (y != gimp_image.height - 1) {
                                r_b = gimp_image.pixel_data[(x + (y + 1) * gimp_image.width) * 4];
                                g_b = gimp_image.pixel_data[(x + (y + 1) * gimp_image.width) * 4 + 1];
                                b_b = gimp_image.pixel_data[(x + (y + 1) * gimp_image.width) * 4 + 2];
                                a_b = gimp_image.pixel_data[(x + (y + 1) * gimp_image.width) * 4 + 3];
                        }

                        uint32_t out = alpha_blend_rgba(
                                rgba(0,0,0,TERM_DEFAULT_OPAC),
                                premultiply(rgba(r_t, g_t, b_t, a_t)));

                        uint32_t back = alpha_blend_rgba(
                                rgba(0,0,0,TERM_DEFAULT_OPAC),
                                premultiply(rgba(r_b, g_b, b_b, a_b)));

                        /* Are both cells transparent? */
                        if (a_t == 0 && a_b == 0) {
                                reset();
                                printf(" ");
                                continue;
                        }

                        if (a_b == 0) {
                                reset();
                                foreground_color(out);
                                printf("▀");
                        } else if (a_t == 0) {
                                reset();
                                foreground_color(back);
                                printf("▄");
                        } else {
                                foreground_color(back);
                                background_color(out);
                                printf("▄");
                        }
                }
                if (j < i) {
                        print_thing(j);
                        j++;
                } else {
                        printf("\033[0m\n");
                }
        }
                while (j < i) {
                for (int x = 0; x < (int)gimp_image.width; x++) {
                        printf(" ");
                }
                print_thing(j);
                j++;
        }
}

```
