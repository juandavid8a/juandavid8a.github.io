---
layout: post
title:  "Cómo crear un Expert Advisor metatrader 5"
description: "Cómo crear un Expert Advisor para metatrader 5"
comments: true
category: Tutoriales
tags: Tutoriales Trucos
youtube: https://youtu.be/ScwqUMKhNm4
---
Código paso a paso para crear un expert advisor que nos ayude a administrar las posiciones que vamos abriendo manualmente.

En <a target="_blank" href="{{ page.youtube }}">mi canal de youtube</a> hay un video del paso a paso:
 
1. Crear un nuevo proyecto.

2. Crear variables:
   - globalTrailing
   - globaltrailingStop
   - globalStopLoss
   - globalSpread

4. Copiar codigo
```C#
//+------------------------------------------------------------------+
//|                                                      vikingo.mq5 |
//|                                  Copyright 2023, MetaQuotes Ltd. |
//|                                             https://www.mql5.com |
//+------------------------------------------------------------------+
#property copyright "Copyright 2023, MetaQuotes Ltd."
#property link      "https://www.mql5.com"
#property version   "1.00"

#include <Trade\Trade.mqh>
CTrade trade;

//--- input parameters
input int globalTrailing;
input int globalTrailingStop;
input int globalStopLoss;
input int globalSpread;

bool     test = false;
bool     globalPositionInit = true;
long     globalPositionTrailing;
double   globalTickValue;
ulong    globalTicket;
string   globalSymbol;
string   globalPositionSymbol;
long     globalPositionType;
long     globalPositionOpen;
long     globalPositionStop;
double   globalGross;
long     globalPrice;
//+------------------------------------------------------------------+
//| Expert initialization function                                   |
//+------------------------------------------------------------------+
int OnInit()
  {
//---
   globalSymbol = Symbol();
   globalTickValue = SymbolInfoDouble(Symbol(), SYMBOL_TRADE_TICK_VALUE);
//---
   return(INIT_SUCCEEDED);
  }

//+------------------------------------------------------------------+
//| Expert tick function                                             |
//+------------------------------------------------------------------+
void OnTick()
  {
   if(test)
     {
      ulong ticket = trade.Buy(0.01, Symbol(), SymbolInfoDouble(Symbol(), SYMBOL_ASK));
      //ulong ticket = trade.Sell(0.01, Symbol(), SymbolInfoDouble(Symbol(), SYMBOL_BID));
      test = false;
     }
   int total = PositionsTotal();
   if(total > 0)
     {
      globalTicket = PositionGetTicket(0);
      globalGross = PositionGetDouble(POSITION_PROFIT);
      globalPositionSymbol = PositionGetString(POSITION_SYMBOL);
      globalPositionOpen = priceConvert(PositionGetDouble(POSITION_PRICE_OPEN));
      globalPositionType = PositionGetInteger(POSITION_TYPE);

      if(globalPositionType == POSITION_TYPE_SELL)
        {
         globalPrice = priceConvert(SymbolInfoDouble(globalPositionSymbol, SYMBOL_ASK));
         if(globalPositionInit)
           {
            globalPositionStop = globalPositionOpen + globalStopLoss;
            globalPositionTrailing = globalPositionOpen - globalTrailing - globalSpread;
            globalPositionInit = false;
           }

         if(globalPrice <= globalPositionTrailing && globalGross > 0)
           {
            globalPositionTrailing = globalPrice - globalTrailing;
            globalPositionStop = globalPrice + globalTrailingStop;
           }

         if(globalPrice >= globalPositionStop)
           {
            bool result = trade.PositionClose(globalTicket);
            if(result)
              {
               resetInit();
              }
            else
              {
               Print("No se pudo cerrar la posición de venta");
              }
           }
        }
      else
         if(globalPositionType == POSITION_TYPE_BUY)
           {
            globalPrice = priceConvert(SymbolInfoDouble(globalPositionSymbol, SYMBOL_BID));
            if(globalPositionInit)
              {
               globalPositionStop = globalPositionOpen - globalStopLoss;
               globalPositionTrailing = globalPositionOpen + globalTrailing + globalSpread;
               globalPositionInit = false;
              }

            if(globalPrice >= globalPositionTrailing && globalGross > 0)
              {
               globalPositionTrailing = globalPrice + globalTrailing;
               globalPositionStop = globalPrice - globalTrailingStop;
              }

            if(globalPrice <= globalPositionStop)
              {
               bool result = trade.PositionClose(globalTicket);
               if(result)
                 {
                  resetInit();
                 }
               else
                 {
                  Print("No se pudo cerrar la posición de compra");
                 }
              }
           }
      Print("Ticket: ", globalTicket, " Apertura: ", globalPositionOpen, " Profit: ", globalGross, " Precio: ", globalPrice, " Trail: ", globalPositionTrailing, " Stop: ", globalPositionStop);
     }
   else
     {
      resetInit();
     }
  }

//+------------------------------------------------------------------+
//| PRICE CONVERT                                                    |
//+------------------------------------------------------------------+
long priceConvert(double price)
  {
   string price_str = DoubleToString(price, 5);
   string parts[];
   int count = StringSplit(price_str, '.', parts);
   if(StringLen(parts[1])==4)
     {
      parts[1] = parts[1] + "0";
     }
   long price_int = StringToInteger(parts[0] + parts[1]);
   return price_int;
  }
//+------------------------------------------------------------------+

//+------------------------------------------------------------------+
//| RESET INIT                                                       |
//+------------------------------------------------------------------+
void resetInit()
  {
   globalPositionInit = true;
   globalPositionTrailing = 0;
   globalTickValue = 0;
   globalTicket = 0;
   globalSymbol = "";
   globalPositionSymbol = "";
   globalPositionType = 0;
   globalPositionOpen = 0;
   globalPositionStop = 0;
   globalGross = 0;
   globalPrice = 0;
  }
//+------------------------------------------------------------------+

//+------------------------------------------------------------------+
//| UPDATE STOPLOSS                                                  |
//+------------------------------------------------------------------+
void setStopLoss()
  {
   double priceOpen = PositionGetDouble(POSITION_PRICE_OPEN);
   double tickSize = SymbolInfoDouble(_Symbol, SYMBOL_TRADE_TICK_SIZE);
   double newStopLoss = priceOpen - globalStopLoss * tickSize;
   trade.OrderModify(globalTicket, 0, priceOpen, newStopLoss, 0, CLR_NONE);
  }
//+------------------------------------------------------------------+
``` 
