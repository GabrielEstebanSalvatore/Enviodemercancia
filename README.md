# Enviodemercancia

using System;

namespace ENVÍO_DE_MERCANCÍAS
{
    class Program
    {
        static void Main(string[] args)
        {
            decimal pesoMercancia, valorMercancia, valorTarifa,
                valorPromocion, valorDescuento;
            string esLunes, tipoPago;

            PedirDatos(out pesoMercancia, out valorMercancia, out esLunes, out tipoPago);
            valorTarifa = CalcularTarifa(pesoMercancia);
            valorDescuento = CalcularDescuento(valorMercancia, valorTarifa);
            valorPromocion = CalcularPromocion(esLunes, tipoPago, valorMercancia, valorTarifa);
            MostrarResultados(valorTarifa, valorDescuento, valorPromocion);
        }

        static void PedirDatos(out decimal pesoMercancia,
                               out decimal valorMercancia,
                               out string esLunes,
                               out string tipoPago)
        {
            Console.Write("Peso mercancia____________________?");
            pesoMercancia = Convert.ToDecimal(Console.ReadLine());
            Console.Write("valor mercancia___________________?");
            valorMercancia = Convert.ToDecimal(Console.ReadLine());
            do {
                Console.Write("Es lunes [S]i, [N]o_______________?");
                esLunes = Console.ReadLine();
                esLunes = esLunes.ToUpper();

                if (esLunes != "S" && esLunes != "N")
                {
                    Console.WriteLine("!Has ingresado un valor incorrecto!");
                }
            } while (esLunes != "S" && esLunes != "N");

            do
            {
                Console.Write("Tipo de pago [E]fectivo, [T]arjeta?");
                tipoPago = Console.ReadLine();
                tipoPago = tipoPago.ToUpper();

                if (tipoPago != "E" && tipoPago != "T")
                {
                    Console.WriteLine("!Has ingresado un valor incorrecto!");
                }

            } while (tipoPago != "E" && tipoPago != "T");
        }

        static decimal CalcularTarifa(decimal pesoMercancia)
        {
            if (pesoMercancia < 100) return 20000;
            if (pesoMercancia <= 150) return 25000;
            if (pesoMercancia <= 200) return 30000;
            return 35000 + (pesoMercancia - 200) / 10 * 2000;
        }
        static decimal CalcularDescuento(decimal valorMercancia, decimal valorTarifa)
        {
            if (valorMercancia < 300000) return 0;
            if (valorMercancia <= 600000) return valorTarifa * 0.1M;
            if (valorMercancia <= 1000000) return valorTarifa * 0.2M;
            return valorTarifa * 0.3M;
        }

        static decimal CalcularPromocion(string esLunes, string tipoPago, 
            decimal valorMercancia, decimal valorTarifa)
        {
            if (esLunes == "S" && tipoPago == "T") return valorTarifa * 0.5M;
            if (tipoPago == "E" && valorMercancia > 1000000) return valorTarifa * 0.4M;
            return 0;
        }
        static void MostrarResultados(decimal valorTarifa,decimal valorDescuento, decimal valorPromocion)
        {
            decimal totalAPagar;
            Console.WriteLine("tarifa       : ${0,12:N0}", valorTarifa);
            if (valorDescuento > valorPromocion)
            {
                Console.WriteLine("Descuento    : ${0,12:N0}", valorDescuento);
                totalAPagar = valorTarifa - valorDescuento;
            }
            else
            { 
                Console.WriteLine("Promoción    : ${0,12:N0}", valorPromocion);
                totalAPagar = valorTarifa - valorPromocion;
            }
            Console.WriteLine("Total a pagar: ${0,12:N0}", totalAPagar);
            Console.ReadKey();
           

        }
    }
}
