#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct BankAccount {
    char name[100];
    int accountNumber;
    float balance;
};

void createAccount(struct BankAccount *accounts, int *numAccounts) {
    printf("Hesap olusturuluyor...\n");
    
    printf("Ad: ");
    scanf("%s", accounts[*numAccounts].name);
    
    printf("Hesap Numarasi: ");
    scanf("%d", &accounts[*numAccounts].accountNumber);
    
    printf("Baslangic Bakiyesi: ");
    scanf("%f", &accounts[*numAccounts].balance);
    
    (*numAccounts)++;
    
    printf("Hesap olusturuldu.\n\n");
}

void displayAccounts(struct BankAccount *accounts, int numAccounts) {
    printf("Hesaplar:\n");
    for (int i = 0; i < numAccounts; i++) {
        printf("Hesap No: %d\n", accounts[i].accountNumber);
        printf("Ad: %s\n", accounts[i].name);
        printf("Bakiye: %.2f\n\n", accounts[i].balance);
    }
}

void deposit(struct BankAccount *accounts, int numAccounts, int accountNumber) {
    float amount;
    int found = 0;
    
    for (int i = 0; i < numAccounts; i++) {
        if (accounts[i].accountNumber == accountNumber) {
            printf("Yatirilacak miktar: ");
            scanf("%f", &amount);
            
            accounts[i].balance += amount;
            printf("Yatirim tamamlandi. Guncel bakiye: %.2f\n\n", accounts[i].balance);
            
            found = 1;
            break;
        }
    }
    
    if (!found) {
        printf("Hesap bulunamadi.\n\n");
    }
}

void withdraw(struct BankAccount *accounts, int numAccounts, int accountNumber) {
    float amount;
    int found = 0;
    
    for (int i = 0; i < numAccounts; i++) {
        if (accounts[i].accountNumber == accountNumber) {
            printf("Cekilecek miktar: ");
            scanf("%f", &amount);
            
            if (accounts[i].balance >= amount) {
                accounts[i].balance -= amount;
                printf("Cekim tamamlandi. Guncel bakiye: %.2f\n\n", accounts[i].balance);
            } else {
                printf("Yetersiz bakiye.\n\n");
            }
            
            found = 1;
            break;
        }
    }
    
    if (!found) {
        printf("Hesap bulunamadi.\n\n");
    }
}

int main() {
    struct BankAccount accounts[100];
    int numAccounts = 0;
    
    FILE *file = fopen("accounts.txt", "r");
    if (file != NULL) {
        fread(&numAccounts, sizeof(int), 1, file);
        fread(accounts, sizeof(struct BankAccount), numAccounts, file);
        fclose(file);
    }
    
    int choice;
    int accountNumber;
    
    while (1) {
        printf("Banka Hesap Yonetimi Sistemi\n");
        printf("1. Hesap Olustur\n");
        printf("2. Hesaplari Goruntule\n");
        printf("3. Para Yatir\n");
        printf("4. Para Cek\n");
        printf("5. Cikis\n");
        printf("Seciminizi yapin: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1:
                createAccount(accounts, &numAccounts);
                break;
                
            case 2:
                displayAccounts(accounts, numAccounts);
                break;
                
            case 3:
                printf("Hesap Numarasi: ");
                scanf("%d", &accountNumber);
                deposit(accounts, numAccounts, accountNumber);
                break;
                
            case 4:
                printf("Hesap Numarasi: ");
                scanf("%d", &accountNumber);
                withdraw(accounts, numAccounts, accountNumber);
                break;
                
            case 5:
                file = fopen("accounts.txt", "w");
                if (file != NULL) {
                    fwrite(&numAccounts, sizeof(int), 1, file);
                    fwrite(accounts, sizeof(struct BankAccount), numAccounts, file);
                    fclose(file);
                }
                
                printf("Programdan cikiliyor...\n");
                exit(0);
            
            default:
                printf("Gecersiz secim. Tekrar deneyin.\n\n");
                break;
        }
    }
    
    return 0;
}
