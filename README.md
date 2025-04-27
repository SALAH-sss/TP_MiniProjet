#include <stdio.h>
#include <string.h>
#include <stdlib.h>
struct Account {
    char name[30];
    char number[20];
    char password[20];
    float balance;
};
void createAccount(){
    struct Account acc;
    printf("Name:");
    scanf("%s",acc.name);
    printf("Account number:");
    scanf("%s",acc.number);
    printf("Password:");
    scanf("%s",acc.password);
    printf("Initial deposit:");
    scanf("%f",&acc.balance);
    FILE *f =fopen("accounts.txt","a");
    fprintf(f, "%s %s %s %.2f\n", acc.name, acc.number, acc.password, acc.balance);
    fclose(f);
    printf("Account created\n");
}
int login(char current[]) {
    char num[20], pass[20];
    printf("Account number: ");
    scanf("%s", num);
    printf("Password: ");
    scanf("%s", pass);
    FILE *f = fopen("accounts.txt", "r");
    struct Account acc;
    while (fscanf(f, "%s %s %s %f", acc.name, acc.number, acc.password, &acc.balance) != EOF) {
        if (strcmp(num, acc.number) == 0 && strcmp(pass, acc.password) == 0) {
            strcpy(current, acc.number);
            fclose(f);
            printf("Login successful\n");
            return 1;
}}
    fclose(f);
    printf("Login failed\n");
    return 0;
}

void deposit(char current[]){
    float amount;
    printf("Amount to deposit: ");
    scanf("%f", &amount);
    FILE *f = fopen("accounts.txt", "r");
    FILE *temp = fopen("temp.txt", "w");
    struct Account acc;
    while (fscanf(f, "%s %s %s %f", acc.name, acc.number, acc.password, &acc.balance) != EOF) {
        if (strcmp(acc.number, current) == 0) {
            acc.balance += amount;
        }
        fprintf(temp, "%s %s %s %.2f\n", acc.name, acc.number, acc.password, acc.balance);
    }
    fclose(f);
    fclose(temp);
    remove("accounts.txt");
    rename("temp.txt", "accounts.txt");
    printf("Deposit done\n");
}

void transferMoney(char current[]) {
    char to[20];
    float amount;
    printf("Transfer to: ");
    scanf("%s", to);
    printf("Amount: ");
    scanf("%f", &amount);
    FILE *f = fopen("accounts.txt", "r");
    FILE *temp = fopen("temp.txt", "w");
    struct Account acc;
    while (fscanf(f, "%s %s %s %f", acc.name, acc.number, acc.password, &acc.balance) != EOF) {
        if (strcmp(acc.number, current) == 0 && acc.balance >= amount) {
            acc.balance -= amount;
        }
        if (strcmp(acc.number, to) == 0) {
            acc.balance += amount;
        }
        fprintf(temp, "%s %s %s %.2f\n", acc.name, acc.number, acc.password, acc.balance);
    }
    fclose(f);
    fclose(temp);
    remove("accounts.txt");
    rename("temp.txt", "accounts.txt");
    printf("Transfer done\n");
}

void checkBalance(char current[]) {
    FILE *f = fopen("accounts.txt", "r");
    struct Account acc;
    while (fscanf(f, "%s %s %s %f", acc.name, acc.number, acc.password, &acc.balance) != EOF) {
        if (strcmp(acc.number, current) == 0) {
            printf("Balance: %.2f\n", acc.balance);
        }
    }
    fclose(f);
}

void deleteAccount(char current[]) {
    FILE *f = fopen("accounts.txt", "r");
    FILE *temp = fopen("temp.txt", "w");
    struct Account acc;
    while (fscanf(f, "%s %s %s %f", acc.name, acc.number, acc.password, &acc.balance) != EOF) {
        if (strcmp(acc.number, current) == 0 && acc.balance == 0) {
            continue;
        }
        fprintf(temp, "%s %s %s %.2f\n", acc.name, acc.number, acc.password, acc.balance);
    }
    fclose(f);
    fclose(temp);
    remove("accounts.txt");
    rename("temp.txt", "accounts.txt");
    printf("Account deleted\n");
}
int main() {
    int choice;
    char current[20];
    while (1{
        printf("\n1. Create Account\n");
        printf("2. Login\n");
        printf("3. Exit\n");
        printf("Choice: ");
        scanf("%d", &choice);
        if (choice == 1) {
            createAccount();
        } else if (choice == 2) {
            if (login(current)) {
                int ch;
                while (1){
                    printf("\n1. Deposit\n");
                    printf("2. Transfer\n");
                    printf("3. Check Balance\n");
                    printf("4. Delete Account\n");
                    printf("5. Logout\n");
                    printf("Choice: ");
                    scanf("%d", &ch);
                    if (ch == 1) deposit(current);
                    else if (ch==2) transferMoney(current);
                    else if (ch==3) checkBalance(current);
                    else if (ch==4) deleteAccount(current);
                    else if (ch==5) break;
    }
    }
    }else{
break;
}
}
    return 0;
}
