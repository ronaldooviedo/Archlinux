# ! / usr / bin / perl
# #
# Copyright 2014 Pierre Mavro <deimos@deimos.fr>
# Copyright 2014 Vivien Didelot <vivien@didelot.org>
# #
# Con licencia bajo los términos de GNU GPL v3, o cualquier versión posterior.
# #
# Este script está destinado a usarse con i3blocks. Analiza la salida del "acpi"
# comando (a menudo proporcionado por un paquete del mismo nombre) para leer el estado de
# la batería y, finalmente, su tiempo restante (a plena carga o descarga).
# #
# El color cambiará gradualmente en un porcentaje inferior al 85%, y la urgencia
# (código de salida 33) se establece si queda menos del 5% restante.

uso estricto;
usar advertencias;
use utf8;

my  $ acpi ;
mi  $ estado ;
mi  $ por ciento ;
my  $ ac_adapt ;
my  $ full_text ;
mi  $ texto_corto ;
my  $ bat_number = $ ENV { BAT_NUMBER } || 0;
my  $ label = $ ENV { LABEL } || " " ;

# lee la primera línea de la salida del comando "acpi"
abrir (ACPI, " acpi -b 2> / dev / null | grep 'Batería $ bat_number ' | " ) o  morir ;
$ acpi = <ACPI>;
cerrar (ACPI);

# falla en salida inesperada
if ( no  definido ( $ acpi )) {
    # no imprima nada en stderr si no hay batería
    salida (0);
}
elsif ( $ acpi ! ~ / : ([ \ w \ s ] +), ( \ d +)% / ) {
	morir  " $ acpi \ n " ;
}

$ estado = $ 1 ;
$ por ciento = $ 2 ;
$ full_text = " $ label $ percent % " ;

if ( $ status  eq  ' Descargando ' ) {
	$ full_text . = ' DIS ' ;
} elsif ( $ status  eq  ' Carga ' ) {
	$ full_text . = ' CHR ' ;
} elsif ( $ status  eq  ' Desconocido ' ) {
	abrir (AC_ADAPTER, " acpi -a | " ) o  morir ;
	$ ac_adapt = <AP_ADAPTER>;
	cerrar (AC_ADAPTER);

	if ( $ ac_adapt = ~ / : ([ \ w -] +) / ) {
		$ ac_adapt = $ 1 ;

		if ( $ ac_adapt  eq  ' en línea ' ) {
			$ full_text . = ' CHR ' ;
		} elsif ( $ ac_adapt  eq  ' fuera de línea ' ) {
			$ full_text . = ' DIS ' ;
		}
	}
}

$ short_text = $ full_text ;

if ( $ acpi = ~ / ( \ d \ d : \ d \ d ): / ) {
	$ full_text . = " ( $ 1 ) " ;
}

# imprimir texto
imprime  " $ full_text \ n " ;
imprimir  " $ texto_corto \ n " ;

# considere el color y la bandera urgente solo al alta
if ( $ status  eq  ' Descargando ' ) {

	si ( $ por ciento <20) {
		imprime  " # FF0000 \ n " ;
	} elsif ( $ por ciento <40) {
		imprima  " # FFAE00 \ n " ;
	} elsif ( $ por ciento <60) {
		imprime  " # FFF600 \ n " ;
	} elsif ( $ por ciento <85) {
		imprime  " # A8FF00 \ n " ;
	}

	if ( $ por ciento <5) {
		salida (33);
	}
}

salida (0);