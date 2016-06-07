#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

FILE *arquivo;
int op;
int i, b;
struct ficha
{
    char nome[50];
    char email[50];
    int cpf;
    int numero;
}cad;

void cabecalho(char *titulo)
{
    system("cls");
    printf("\n\t\t[%s]\n\n", titulo);
}

void cadastrar();
void clientes();
void consultar();
void alterar();
void excluir();


int main()
{
    int opcao;
    do
    {
        cabecalho("Menu");
        printf("1 - Cadastrar Novo Cliente\n");
        printf("2 - Clientes\n");
        printf("3 - Consultar Ficha de Cliente\n");
        printf("4 - Alterar Cliente\n");
        printf("5 - Excluir Cliente\n");
        printf("Digite 0 para sair: ");
        scanf("%d", &opcao);

        switch(opcao)
        {
        case 1:
            cadastrar();
            break;

        case 2:
            clientes();
            break;

        case 3:
            consultar();
            break;

        case 4:
            alterar();
            break;

        case 5:
            excluir();
            break;

        default:
            if(opcao != 0)
                printf("\nOpcao invalida\n");
        }
    }while(opcao != 0);

    getchar();
    return 0;
}
void cadastrar()
{
    int n = 1;
    int flag = 0;

    do
    {
        cabecalho("Cadastrar");

        if((arquivo = fopen("cadastro.bin", "ab")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }
        fclose(arquivo);

        if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }

        fseek(arquivo, 0, SEEK_END);
        b = ftell(arquivo) / sizeof(struct ficha);
        for(i = 0; i < b; i++)
        {
            fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
            fread(&cad, sizeof(struct ficha), 1, arquivo);

            if(cad.numero > flag)
                flag = cad.numero;
        }
        fclose(arquivo);

        printf("Digite o nome: ");
        scanf("%s", cad.nome);
        printf("\nDigite o email: ");
        scanf("%s", cad.email);
        printf("\nDigite o CPF: ");
        scanf("%d", &cad.cpf);

        if(flag >= n)
            n = ++flag;
        cad.numero = n;

        if((arquivo = fopen("cadastro.bin", "ab")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }

        fwrite(&cad, sizeof(struct ficha), 1, arquivo);
        fclose(arquivo);

        printf("Digite 0 para sair: ");
        scanf("%d", &op);
    }while(op != 0);
}

void clientes()
{
    fflush(stdin);
    do
    {
        cabecalho("Listar");

        if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }
        fseek(arquivo, 0, SEEK_END);
        b = ftell(arquivo) / sizeof(struct ficha);
        for(i = 0; i < b; i++)
        {
            fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
            fread(&cad, sizeof(struct ficha), 1, arquivo);
            printf("Numero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
        }

        fclose(arquivo);

        printf("\nDigite 0 para sair: ");
        scanf("%d", &op);

    }while(op != 0);
}

void consultar()
{
    fflush(stdin);
    int pesquisa_cpf, pesquisa_cad;
    char pesquisa_email[50];
    char pesquisa_nome[50];

    do
    {
        cabecalho("Consultar");

        printf("1 - Por CPF\n2 - Por Email\n3 - Por numero de cadastro\n4 - Por nome\n");
        printf("Digite 0 para sair: ");
        scanf("%d", &op);

        switch(op)
        {
        case 1:
            printf("\nDigite o CPF: ");
            scanf("%d", &pesquisa_cpf);

            if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
            {
                printf("Erro!\n");
                exit(1);
            }

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(cad.cpf == pesquisa_cpf)
                {
                    printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
                    break;
                }
            }
                fclose(arquivo);
            break;

        case 2:
            printf("\nDigite o email: ");
            scanf("%s", pesquisa_email);

            if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
            {
                printf("Erro!\n");
                exit(1);
            }

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(!(strcmp(cad.email, pesquisa_email)))
                {
                    printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
                    break;
                }
            }
                fclose(arquivo);
            break;

        case 3:
            printf("\nDigite o numero de cadastro: ");
            scanf("%d", &pesquisa_cad);

            if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
            {
                printf("Erro!\n");
                exit(1);
            }

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(cad.numero == pesquisa_cad)
                {
                    printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d", cad.numero, cad.nome, cad.email, cad.cpf);
                    break;
                }
            }
                fclose(arquivo);
            break;

        case 4:
            printf("\nDigite o nome: ");
            scanf("%s", pesquisa_nome);

            if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
            {
                printf("Erro!\n");
                exit(1);
            }

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(!(strcmp(cad.nome, pesquisa_nome)))
                {
                    printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
                    break;
                }
            }

            fclose(arquivo);

            break;

        default:
            if(op != 0)
                printf("\nOpcao invalida");
        }

        printf("\nDigite 0 para sair: ");
        scanf("%d", &op);
    }while(op != 0);
}

void alterar()
{
    fflush(stdin);
    FILE *reserva;
    char resp;
    do
    {
        cabecalho("Alterar");
        printf("Digite o numero de cadastro que deseja alterar: ");
        scanf("%d", &op);

        if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }

        fseek(arquivo, 0, SEEK_END);
        b = ftell(arquivo) / sizeof(struct ficha);

        for(i = 0; i < b; i++)
        {
            fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
            fread(&cad, sizeof(struct ficha), 1, arquivo);

            if(cad.numero == op)
            {
                printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
                break;
            }
        }
            fflush(stdin);
            printf("\nConfirma alteracao? (s/n): ");
            resp = tolower(getchar());

        if(resp == 's')
        {
            if((reserva = fopen("reserva.bin", "wb")) == NULL)
                {
                    printf("Erro!\n");
                    exit(1);
                }
            rewind(arquivo);
            rewind(reserva);

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(cad.numero == op)
                {
                    printf("Digite o nome: ");
                    scanf("%s", cad.nome);
                    printf("\nDigite o email: ");
                    scanf("%s", cad.email);
                    printf("\nDigite o CPF: ");
                    scanf("%d", &cad.cpf);
                }

                fwrite(&cad, sizeof(struct ficha), 1, reserva);

            }
            fclose(arquivo);
            fclose(reserva);
            remove("cadastro.bin");
            rename("reserva.bin", "cadastro.bin");
        }
        else if(resp == 'n')
            fclose(arquivo);

        printf("\nDigite 0 para sair: ");
        scanf("%d", &op);
    }while(op != 0);
}

void excluir()
{
    fflush(stdin);
    FILE *reserva;
    char resp;
    do
    {
        cabecalho("Excluir");
        printf("Digite o numero de cadastro que deseja excluir: ");
        scanf("%d", &op);

        if((arquivo = fopen("cadastro.bin", "rb")) == NULL)
        {
            printf("Erro!\n");
            exit(1);
        }

        fseek(arquivo, 0, SEEK_END);
        b = ftell(arquivo) / sizeof(struct ficha);

        for(i = 0; i < b; i++)
        {
            fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
            fread(&cad, sizeof(struct ficha), 1, arquivo);

            if(cad.numero == op)
            {
                printf("\nNumero de cadastro: %d\nNome: %s\nEmail: %s\nCPF: %d\n", cad.numero, cad.nome, cad.email, cad.cpf);
                break;
            }
        }
            fflush(stdin);
            printf("\nConfirma exclusao? (s/n): ");
            resp = tolower(getchar());

        if(resp == 's')
        {
            if(!(reserva = fopen("reserva.bin", "wb")))
                {
                    printf("Erro!\n");
                    exit(1);
                }
            rewind(arquivo);
            rewind(reserva);

            fseek(arquivo, 0, SEEK_END);
            b = ftell(arquivo) / sizeof(struct ficha);

            for(i = 0; i < b; i++)
            {
                fseek(arquivo, i * sizeof(struct ficha), SEEK_SET);
                fread(&cad, sizeof(struct ficha), 1, arquivo);

                if(cad.numero != op)
                {
                    fwrite(&cad, sizeof(struct ficha), 1, reserva);
                }
            }
            fclose(arquivo);
            fclose(reserva);
            remove("cadastro.bin");
            rename("reserva.bin", "cadastro.bin");
        }
        else if(resp == 'n')
            fclose(arquivo);

        printf("\nDigite 0 para sair: ");
        scanf("%d", &op);
    }while(op != 0);

}
