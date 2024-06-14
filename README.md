1)a)podemos notar con respecto al tiempo de ejecución que es variable e impredecible, ya que al ejecutarlos varias veces a los dos el tiempo exacto que retornaba cada código cambiaba sin seguir ningún patron especifico.

b) pudimos comprobar que los tiempos de ejecucion son distintos ya que al ejecutarlos el tiempo sigue siendo imprecendible y no son iguales.

c)Lo que paso fue que el tiempo y el valor final, fueron variando en cada resultado de ejecución, nuevamente como el inciso a) no se pudo predecir un patron o un porque de esta variación. Si pude notar que antes de descomentar las líneas de código el valor final era siempre 0 y el tiempo siempre cambiaba de una ejecución a otra. Una vez descomentadas, todo era variable.


1)#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

#define NUMBER_OF_THREADS 2
#define CANTIDAD_INICIAL_HAMBURGUESAS 20
pthread_mutex_t llave;

int cantidad_restante_hamburguesas = CANTIDAD_INICIAL_HAMBURGUESAS;
int turno = 0;

void *comer_hamburguesa(void *tid)
{
	while (1 == 1) 
	{
		while (turno != (int)tid)

			if (cantidad_restante_hamburguesas > 0)
			{
				printf("Hola! soy el hilo(comensal) %d, me voy a comer una hamburguesa! Todavía quedan %d \n", (int)tid, cantidad_restante_hamburguesas);
				cantidad_restante_hamburguesas--;
				
			}
			else
			{
				printf("SE TERMINARON LAS HAMBURGUESAS :( \n");
				turno = (turno + 1)% NUMBER_OF_THREADS;
				pthread_exit(NULL);
			}
			turno = (turno + 1) % NUMBER_OF_THREADS;

	}
}

int main(int argc, char *argv[])
{
	pthread_t threads[NUMBER_OF_THREADS];
	int status, i, ret;

	for (int i = 0; i < NUMBER_OF_THREADS; i++)
	{
		printf("Hola!, soy el hilo principal. Estoy creando el hilo %d \n", i);
		status = pthread_create(&threads[i], NULL, comer_hamburguesa, (void *)i);
		if (status != 0)
		{
			printf("Algo salio mal, al crear el hilo recibi el codigo de error %d \n", status);
			exit(-1);
		}
	}

	for (i = 0; i < NUMBER_OF_THREADS; i++)
	{
		void *retval;
		ret = pthread_join(threads[i], &retval);
		if (ret != 0)
		{
			printf("Error en pthread_join: %d\n", ret);
			exit(-1);
		}
	}
	pthread_exit(NULL);
}





2)b)
Turno 1:
El Comensal 1 entra a la zona crítica.
El Comensal 1 se come una hamburguesa. (quedan 7 hamburguesas).
El Comensal 1 sale de la zona crítica.

Turno 2:
El Comensal 2 entra a la zona crítica.
El Comensal 2 se come una hamburguesa. (quedan 6 hamburguesas).
El Comensal 2 sale de la zona crítica.

Turno 3:
El Comensal 1 entra a la zona crítica.
El Comensal 1 se come una hamburguesa. (quedan 5 hamburguesas).
El Comensal 1 sale de la zona crítica.

Turno 4:
El Comensal 2 entra a la zona crítica.
El Comensal 2 se come una hamburguesa. (quedan 4 hamburguesas).
El Comensal 2 sale de la zona crítica.

Turno 5:
El Comensal 1 entra a la zona crítica.
El Comensal 1 se come una hamburguesa. (quedan 3 hamburguesas).
El Comensal 1 sale de la zona crítica.

Turno 6:
El Comensal 2 entra a la zona crítica.
El Comensal 2 se come una hamburguesa. (quedan 2 hamburguesas).
El Comensal 2 sale de la zona crítica.

Turno 7:
El Comensal 1 entra a la zona crítica.
El Comensal 1 se come una hamburguesa. (queda 1 hamburguesa).
El Comensal 1 sale de la zona crítica.

Turno 8:
El Comensal 2 entra a la zona crítica.
El Comensal 2 se come la hamburguesa faltante. (quedan 0 hamburguesas).
El Comensal 2 sale de la zona crítica.



