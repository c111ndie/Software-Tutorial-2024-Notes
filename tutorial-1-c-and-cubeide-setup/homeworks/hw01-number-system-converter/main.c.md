#include <stdio.h>
#include <string.h>
#include <stdint.h>
#include <stdbool.h>

const char* const error_msg_decimal = "Error! That set of number is not a decimal number.\n";
const char* const error_msg_trinary = "Error! That set of number is not a trinary number.\n";
const char* const error_msg_duodecimal = "Error! That set of number is not a duodecimal number.\n";
const char* const error_msg_unsupported_system = "Error! The number system is not supported.\n";

const char* const msg_prompt_number = "Please enter a set of number:\n";
const char* const msg_prompt_current_number_system = "Please enter the current number system:\n";
const char* const msg_prompt_number_system_to_convert = "Please enter the number system you want the set of number be converted to:\n";
const char* const msg_output = "Output=";

int main(){
  
  
    printf("%s", msg_prompt_number);
  
    /**
     * @brief 
     * Get user input for number to be convert
     */

    char n[1000];
    scanf("%s", n);

    printf("%s", msg_prompt_current_number_system);

    /**
     * @brief 
     * Get user input for current number system
     */

    int sys;
    scanf("%d", &sys);
    if (sys == 10){
        for(int i = 0; i < strlen(n); i++){
            if(n[i] < '0' || n[i] > '9'){
                printf("%s", error_msg_decimal);
                return 1;
            }
        }
    }
    else if (sys == 3){
        for(int i = 0; i < strlen(n); i++){
            if(n[i] < '0' || n[i] > '2'){
                printf("%s", error_msg_trinary);
                return 2;
            }
        }
    }
    else if (sys == 12){
        for(int i = 0; i < strlen(n); i++){
            if(!(n[i] >= '0' && n[i] <= '9') && !(n[i] == 'A' || n[i] == 'B')){
                printf("%s", error_msg_duodecimal);
                return 3;
            }
        }
    }
    else{
        printf("%s", error_msg_unsupported_system);
        return 4;
    }
    printf("%s", msg_prompt_number_system_to_convert);

    /**
     * @brief 
     * Get user input for the number system for conversion.
     * Print converted number in format of "Output=12", e.g Output=ABCDEF
     * No space should be inserted in the asnwer "Output=XXX".
     * In case of wrong number system for conversion, please use the above error msgs.
     */

    int csys;
    scanf("%d", &csys);
    int len = strlen(n), digits[strlen(n)];
    for(int i = 0; i < len; i++){
        if (n[i] >= '0' && n[i] <= '9'){
            digits[i] = n[i] - '0'; 
        }
        else{
            if (n[i] == 'A')
                digits[i] = 10;
            else 
                digits[i] = 11;
        }
    }
    int dec = 0, power = 1;
    if (sys != 10){
        if (sys == 3){
            for(int i = len - 1; i >= 0; i--){
                dec += digits[i] * power;
                power *= 3;
            }
        }
        else if (sys == 12){
            for(int i = len - 1; i >= 0; i--){
                dec += digits[i] * power;
                power *= 12;
            }
        }
        if (csys == 10){
            printf("%s", msg_output);
            printf("%d", dec);
            return 5;
        }
    }
    else{
        for(int i = len - 1; i >= 0; i--){
            dec += digits[i] * power;
            power *= 10;
        }
    }
    //convert dec to ternary/duodec
    int index = 0, base = csys;
    char ans[1000];
    while (dec > 0) {
        int remainder = dec % base;
        dec /= base;

        if (base == 12 && remainder >= 10) {
            ans[index++] = 'A' + (remainder - 10);
        } else {
            ans[index++] = '0' + remainder; 
        }
    }
    printf("%s", msg_output);
    for (int i = index - 1; i >= 0; i--) {
        printf("%c", ans[i]);
    }

    return 0;
}
// C programmers never die. They are just cast into void.
