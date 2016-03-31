# proyecto_ventas
#include <stdio.h>
#include<stdlib.h>
#include <windows.h>
struct registro{
   char producto[40];
   float precio[6];
   int cantidad[6];
   struct registro *sig;    
};
typedef struct registro nodo;
int mostrar_menu1(){
    int opcion;
    system("cls");
    printf("\nSistema de ventas");
	printf("\n\tSeleccione el departamento que desee: ");
	printf("\n1.Electronica  ");
	printf("\n2.Hogar  ");
	printf("\n3.Electrodomesticos  ");
	printf("\n4.Videojuegos ");
	printf("\n5.Alimentos ");
    printf("\nElige una de las opciones: ");
    scanf("%d",&opcion);
	return(opcion);
}

int mostrar_menu(){
    int opcion;
    system("cls");
    printf("\nSistema de ventas");
    printf("\n1. Insertar producto.");
    printf("\n2. Mostrar lista.");
    printf("\n3. Borrar producto.");
    printf("\n4. Salir del programa.");
    printf("\nElige una de las opciones: ");
    scanf("%d",&opcion);
    
    return(opcion);
}

void ingresar_producto(nodo *inicio){
    nodo *registro;
    int cont=1;
    char seguir='s';
    registro=inicio;
    do{
       printf("\nProducto %d: ",cont++);
       scanf(" %[^\n]",registro->producto);
    	printf("\nPrecio %d: ",cont++);
       scanf(" %[^\n]",registro->precio);
        printf("\nCantidad inicial %d: ",cont++);
       scanf(" %[^\n]",registro->cantidad);
       printf("\nDeseas añadir otro producto (s/n)?: ");
       scanf(" %c",&seguir);
       if(seguir=='s'){
          registro->sig=(nodo *) malloc(1*sizeof(nodo)); 
          registro=registro->sig;
       }
       else{
          printf("\nFINAL DE LA LISTA");
          registro->sig=0;     
      }
    }while(seguir=='s');
    return;
}
void mostrar_lista(nodo *inicio){
	FILE *doc;
	doc = fopen("khkj1234.txt","a+");	
   nodo *registro;
   int cont=1;
   registro=inicio;
   printf("\n************ LISTA ************");
   do{
   	printf("\n\tProducto %d: %s",cont++,registro->producto); 
	printf("\n\tPrecio %d: %s",cont++,registro->precio);   
	printf("\n\tCantidad %d: %s",cont++,registro->cantidad);
    fprintf(doc,"\n\tProducto: %s",registro->producto); 
	fprintf(doc,"\n\tPrecio: %d",registro->precio);   
	fprintf(doc,"\n\tCantidad: %d",registro->cantidad);
    registro=registro->sig;
   }while(registro!=0);
   printf("\n\nPara continuar presione cualquier tecla.");
   fflush(stdin);
   getchar();
   return;
}
nodo *borrar_nodo(nodo *inicio,char aux_nombre[]){
   nodo *registro;
   char seguir='s';
   registro=inicio; 
   //En caso de que el nodo que quiero borrar
   //sea el primero
   if(strcmp(registro->producto,aux_nombre)==0)
     inicio=registro->sig;
   else  
   //EN caso de que el nodo que quiero borrar
   //esté entre el primero y el último
   do{
        if(strcmp((registro->sig)->producto,aux_nombre)==0){
            registro->sig=(registro->sig)->sig;
            seguir='n';
        }
        else
            registro=registro->sig; 
    }while(seguir=='s' && registro!=0);
   return(inicio);
}
main(){	
    nodo *inicio;
    char aux_nombre[40],aux_nombre_anterior[40];
    inicio=(nodo *) malloc(1*sizeof(nodo));
    int opcion;
    do{
        opcion=mostrar_menu1();
        opcion=mostrar_menu();
        switch(opcion){
            case 0:
                    printf("\nPrograma finalizado");
                    break;
            case 1: //Leer lista
                    ingresar_producto(inicio);
                    break;
            case 2: //Mostrar lista
                    mostrar_lista(inicio);
                    break;
            case 3://Eliminar nodo
                    printf("\nEscriba el producto que desea borrar de la lista: ");
                    scanf(" %[^\n]",aux_nombre);
                    inicio=borrar_nodo(inicio,aux_nombre);
                    break;
            case 4:
                    break;
            default:
                 printf("\nOpcion incorrecta.");
                 getchar();     
        }
    }while(opcion!=5);
    getchar();
    getchar();
}
