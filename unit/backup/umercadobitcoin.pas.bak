unit UMercadoBitCoin;

//******************************************************************************
//+++++++++++++++ Conexão com a API do Site MercadoBitCoin.com.br ++++++++++++++
//******************************************************************************
interface

uses Dateutils, SysUtils, HTTPSend, ssl_openssl, CHash, Classes, fpjsonrtti,fpjson,jsonparser, Forms, Dialogs,
     variants;

type

  TMethodTApi = ( taGetInfo, taOrderList, taTrade, taCancelOrder );
  TMethodApi  = ( apTicker, apOrderBook, apTrade );
  TCryptoCoin = ( BitCoin, LiteCoin );
  TTradeType  = ( Buy, Sell) ;
  TStatusOrderList = ( solActive, solCanceled, solCompleted );
  TPair = ( pBtc_Brl, pLtc_Brl );

  TBaseTradeAPi= class
    private
      FSuccess:Boolean;
      FReturn:TCollection;
      FError:String;
    published
      property success:boolean read FSuccess write FSuccess;
      property return:TCollection read FReturn;
      property error:String read FError write Ferror;
  end;

  TFund = class
    private
      FBtc,
      FLtc,
      FBrl:Real;
    public
      property btc:Real read FBtc write FBtc;
      property ltc:Real read FLtc write FLtc;
      property brl:Real read FBrl write FBrl;

  end;

  TInfoItem = class(TCollectionItem)
    private
      FFund:TFund;
      FopenOrders:Integer;
      FServerTime:int64;
    public
      property fund:TFund read FFund;
      property openOrders:Integer read FopenOrders;
      property serverTime:Int64 read FServerTime;
  end;

  TInfo = class(TBaseTradeAPi);

  Order = class
    private
      FPrice,
      FVolume:Real;
    published
      property price:Real read FPrice;
      property volume:Real read FVolume;
  end;

  OrderBook = class
    private

      FBids,
      FAsks: array of Order;

      function GetAskCount: Integer;
      function GetBidsCount: Integer;
      function GetAsk(Index: Integer): Order;
      function GetBid(Index: Integer): Order;
    public
      property ask[Index: Integer]:Order read GetAsk;
      property bid[Index: Integer]:Order read GetBid;
    published
      property asksCount:Integer read GetAskCount;
      property bidsCount:Integer read GetBidsCount;
  end;

  TOrderList = class(TBaseTradeAPi);

  TItemOrderListInfo = class
    private
      FStatus:TStatusOrderList;
      FCreated:Int64;
      FPrice,
      FVolume:Real;
      FPair:TPair;
      FType:TTradeType;
    published
      property status:TStatusOrderList read FStatus write FStatus;
      property created:Int64 read FCreated write FCreated;
      property price:Real read FPrice write FPrice;
      property volume:Real read FVolume write FVolume;
      property pair:TPair read FPair write Fpair;
      property typeOrder:TTradeType read Ftype write Ftype;
  end;

  TItemOrderList = class(TCollectionItem)
    private
      FIdOrder:Integer;
      FOrderListInfo:TItemOrderListInfo;
    public
    published
      property id:Integer read FIdOrder;
      property orderListInfo:TItemOrderListInfo read FOrderListInfo write FOrderListInfo;
  end;

  { TTrade }
  TTrades = class(TCollection);

  TTrade = class(TCollectionItem)
  private
    Famount: Real;
    Fdate: Int64;
    Fprice: Real;
    Ftid: Real;
    FtypeTarde: TTradeType;
    procedure Setamount(AValue: Real);
    procedure Setdate(AValue: Int64);
    procedure Setprice(AValue: Real);
    procedure Settid(AValue: Real);
    procedure SettypeTarde(AValue: TTradeType);
  published
    property date:Int64 read Fdate write Setdate;
    property price:Real read Fprice write Setprice;
    property amount:Real read Famount write Setamount;
    property tid:Real read Ftid write Settid;
    property typeTarde:TTradeType read FtypeTarde write SettypeTarde;
  end;

  { Ticker }

  TTicker = class
    private
      FHigh,
      FLow,
      FVolume,
      FLast,
      FBuy,
      FSell:Real;

      FTimeStamp:Int64;

      function GetHigh: Real;
      procedure SetHigh(const Value: Real);
      function GetBuy: Real;
      function GetLast: Real;
      function GetLow: Real;
      function GetSell: Real;
      function GetTimeStamp: Int64;
      function GetVolume: Real;

      procedure SetBuy(const Value: Real);
      procedure SetLast(const Value: Real);
      procedure SetLow(const Value: Real);
      procedure SetSell(const Value: Real);
      procedure SetTimeStamp(const Value: Int64);
      procedure SetVolume(const Value: Real);
    published
      property high:Real read GetHigh write SetHigh;
      property low:Real read GetLow write SetLow;
      property vol:Real read GetVolume write SetVolume;
      property last:Real read GetLast write SetLast ;
      property buy:Real read GetBuy write SetBuy;
      property sell:Real read GetSell write SetSell;
      property date:Int64 read GetTimeStamp write SetTimeStamp;
  end;

  { MercadoBitCoin }

  MercadoBitCoin = class

  private

    FHourAdjustment,
    FPin:Integer;
    FKey,
    FCode:String;

    FLiteCoinTicker,
    FBitCoinTicker:TTicker;

    FInfo:Tinfo;
    FLiteCoinActiveOrderList:TOrderList;

    function GetBitCoinTrade: TTrades;
    function GetInfo: TInfo;
    function GetLiteCoinActiveOrderList: TOrderList;
    function GetLiteCoinTrade: TTrades;
    function GetTonce:Integer;overload;
    function GenerateSign(method:TMethodTApi; tonce:Integer):String;
    function MethodTradeApi2String(method:TMethodTApi):String;

    procedure ConfigureHttpRequest(httpRequest:THttpSend;method:TMethodTApi);overload;
    procedure ConfigureHttpRequest(httpRequest:THttpSend;method:TMethodTApi; pair:String;tipo:String='';volume:String='';price:String='';orderId:String='');overload;


    function GetLiteCoinOrderBook:String;
    function GetBitCoinOrderBook:String;
    function GetOrderBook(CryptoCoin:TCryptoCoin):String;

    function GetTrades(CryptoCoin:TCryptoCoin):TTrades;overload;
    function GetTrades(CryptoCoin:TCryptoCoin; timeStampStart:Int64 = 0; timeStampFinal:Int64=0):TTrades;overload;

    function GetLiteCoinTicker:TTicker;
    function GetBitCoinTicker:TTicker;

    function GetRequestApi(CryptoCoin:TCryptoCoin; MethodApi:TMethodApi):String;
    function GetRequestTApi(MethodApi:TMethodTApi):String;

    function GetTicker(CryptoCoin:TCryptoCoin):TTicker;
    function MethodApi2String(method:TMethodApi):String;

  public

    constructor Create( Key, Code :String; Pin:Integer);overload;
    constructor Create;overload;

    destructor Destroy;overload;


  published
    property HourAdjustment:Integer read FHourAdjustment write FHourAdjustment;
    property Key:String read FKey;
    property Code:String read FCode;
    property Pin:Integer read FPin;
    property LiteCoinTicker:TTicker read GetLiteCoinTicker;
    property BitCoinTicker:TTicker read GetBitCoinTicker;
    property BitCoinTrade:TTrades read GetBitCoinTrade;
    property LiteCoinTrade:TTrades read GetLiteCoinTrade;
    property info:TInfo read GetInfo;
    property LiteCoinActiveOrderList:TOrderList read GetLiteCoinActiveOrderList;
end;

implementation

{ MercadoBitCoin }
const
  UriTAPI:String = 'https://www.mercadobitcoin.com.br/tapi/';

  UriAPI:String  = 'https://www.mercadobitcoin.com.br/api/v2/';

  TapiRequestMethod:String='POST';

  ApiRequestMethod:String='GET';

  HttpContentMask:String = #13+#10 + '--%s' + #13+#10 + 'Content-Disposition: form-data; name="%s"' + #13+#10 + #13+#10+'%s';

 { TTrade }

 procedure TTrade.Setamount(AValue: Real);
begin
  if Famount=AValue then Exit;
  Famount:=AValue;
end;

 procedure TTrade.Setdate(AValue: Int64);
begin
  if Fdate=AValue then Exit;
  Fdate:=AValue;
end;

 procedure TTrade.Setprice(AValue: Real);
begin
  if Fprice=AValue then Exit;
  Fprice:=AValue;
end;

 procedure TTrade.Settid(AValue: Real);
begin
  if Ftid=AValue then Exit;
  Ftid:=AValue;
end;

 procedure TTrade.SettypeTarde(AValue: TTradeType);
begin
  if FtypeTarde=AValue then Exit;
  FtypeTarde:=AValue;
end;

{ TBaseObject }

 procedure MercadoBitCoin.ConfigureHttpRequest(httpRequest:THttpSend;method:TMethodTApi; pair:String='';tipo:String='';volume:String='';price:String='';orderId:String='');
var Bound,
    Sign,
    Param:String;
    Params:TStringList;
    i,
    tonce:Integer;

begin

  try

    Params := TStringList.Create;

    Bound := IntToHex(Random(MaxInt), 8) + '_Synapse_boundary';
    tonce:=Self.GetTonce;
    Sign := GenerateSign( method ,tonce);

    Params.Add(Format('method=%s',[Self.MethodTradeApi2String(method)]));
    Params.Add(Format('tonce=%s',[IntToStr(tonce)]));

    case method of
      taOrderList:begin
        Params.Add(Format('pair=%s',['ltc_brl']));
        Params.Add(Format('status=%s',['active']));
      end;

      taTrade:begin
        Params.Add(Format('pair=%s',[pair]));
        Params.Add(Format('type=%s',[tipo]));
        Params.Add(Format('volume=%s',[volume]));
        Params.Add(Format('price=%s',[price]));
      end;

      taCancelOrder:begin
        Params.Add(Format('pair=%s',[pair]));
        Params.Add(Format('order_id=%s',[orderId]));
      end;
    end;

    Param := '';

    for i:=0 to Params.Count-1 do
    begin
      Param := Format(HttpContentMask,[Bound, Params.Names[i], Params.Values[Params.Names[i]]]);
      httpRequest.Document.Write( Pointer(Param)^, Length(Param) );
    end;

    Param := #13+#10 + '--' + Bound + '--' + #13+#10;

    httpRequest.Document.Write( Pointer(Param)^, Length(Param) );

    httpRequest.MimeType := 'multipart/form-data; boundary=' + Bound;
    httpRequest.UserAgent := '';
    httpRequest.Protocol := '1.1' ;
    httpRequest.AddPortNumberToHost := False;

    httpRequest.Headers.Add('Key:'+FKey);
    httpRequest.Headers.Add('Sign:'+Sign);

  finally
    if Assigned(Params) then
      FreeAndNil(Params);
  end;

end;

procedure MercadoBitCoin.ConfigureHttpRequest(httpRequest: THttpSend; method: TMethodTApi);
begin
  Self.ConfigureHttpRequest(httpRequest, method, '','','','','');
end;

constructor MercadoBitCoin.Create(Key, Code: String; Pin:Integer);
begin

  Self.FKey := Key;
  Self.FCode := Code;
  Self.FPin := Pin;

end;

constructor MercadoBitCoin.Create;
begin
  //
end;

destructor MercadoBitCoin.Destroy;
begin

  if Assigned(FLiteCoinTicker) then
    FreeAndNil(FLiteCoinTicker);

  if Assigned(FBitCoinTicker) then
    FreeAndNil(FBitCoinTicker);

  if Assigned(FInfo) then
      FreeAndNil(FInfo);

end;

function MercadoBitCoin.GenerateSign(method:TMethodTApi; tonce:Integer): String;
var hashType:THashType;
    Digest : Array [0..MaxHashDigestSize] of Byte;
begin

  hashType := hashHMAC_SHA512;

  CalculateHash (hashType, Format('%s:%d:%d',[MethodTradeApi2String(method),FPin, tonce]), @Digest, FCode);

  Result := DigestToHex( Digest, GetDigestSize(hashType) );

end;

function MercadoBitCoin.GetBitCoinOrderBook: String;
begin
  Result :=  Self.GetOrderBook( BitCoin );
end;

function MercadoBitCoin.GetBitCoinTicker: TTicker;
begin
  Result := Self.GetTicker( BitCoin );
end;

function MercadoBitCoin.GetLiteCoinOrderBook: String;
begin
  Result := Self.GetOrderBook( LiteCoin );
end;

function MercadoBitCoin.GetLiteCoinTicker: TTicker;
begin
  Result := Self.GetTicker( LiteCoin );
end;

function MercadoBitCoin.GetOrderBook( CryptoCoin: TCryptoCoin): String;
begin
  Result := Self.GetRequestApi(CryptoCoin, apOrderBook);
end;

function MercadoBitCoin.GetRequestApi(CryptoCoin: TCryptoCoin; MethodApi: TMethodApi): String;
var httpRequest:THttpSend;
    DataStream: TStringStream;
    uriMethodApi:String;

begin
  try
    httpRequest := THTTPSend.Create;
    DataStream := TStringStream.Create('');

    uriMethodApi := Self.MethodApi2String(MethodApi);

    if CryptoCoin = LiteCoin then
      uriMethodApi := uriMethodApi +'_litecoin';

    uriMethodApi := uriMethodApi + '/';

    if MethodApi = apTrade then
      uriMethodApi := uriMethodApi + IntToSTr( Self.GetTonce )+'/';

    httpRequest.HTTPMethod( ApiRequestMethod, UriAPI+uriMethodApi);

    DataStream.CopyFrom( httpRequest.Document,0 );

    Result := DataStream.DataString;

  finally
    if Assigned(httpRequest) then
      FreeAndNil(httpRequest);

    if Assigned(DataStream) then
      FreeAndNil(DataStream);
  end;

end;

function MercadoBitCoin.GetRequestTApi(MethodApi: TMethodTApi): String;
var httpRequest:THttpSend;
    DataStream: TStringStream;
    uriMethodApi:String;

begin
  try
    httpRequest := THTTPSend.Create;
    DataStream := TStringStream.Create('');

    uriMethodApi := Self.MethodTradeApi2String(MethodApi);

    uriMethodApi := uriMethodApi + '/';

    Self.ConfigureHttpRequest(httpRequest,MethodApi);

    httpRequest.HTTPMethod( TapiRequestMethod, UriTAPI);

    DataStream.CopyFrom( httpRequest.Document,0 );

    Result := DataStream.DataString;

  finally
    if Assigned(httpRequest) then
      FreeAndNil(httpRequest);

    if Assigned(DataStream) then
      FreeAndNil(DataStream);
  end;


end;

function MercadoBitCoin.GetTicker(CryptoCoin: TCryptoCoin): TTicker;
var tickerRes:TTicker;
    j:TJSONData;
    vstTexto:String;
    parser:TJSONParser;
begin

    vstTexto:= Self.GetRequestApi( CryptoCoin, apTicker );
    try

      try
        parser := TJSONParser.Create(vstTexto);

        j := parser.Parse;

        If Assigned(J) then
        begin
          tickerRes := TTicker.Create;

          tickerRes.high :=J.Items[0].Items[0].AsFloat;
          tickerRes.low  :=J.Items[0].Items[1].AsFloat;
          tickerRes.vol  :=J.Items[0].Items[2].AsFloat;
          tickerRes.last :=J.Items[0].Items[3].AsFloat;
          tickerRes.buy  :=J.Items[0].Items[4].AsFloat;
          tickerRes.sell :=J.Items[0].Items[5].AsFloat;
          tickerRes.date :=J.Items[0].Items[5].AsInt64;
          Result := tickerRes;
        end;

      except
        Application.MessageBox(PChar('Falha no Parser'),'Teste',0);
      end;

    finally
      j.Free;
      parser.Free;
    end;

end;

function MercadoBitCoin.GetTonce: Integer;
begin
  Result := DateTimeToUnix( IncHour(Now, HourAdjustment) ) ;
end;

function MercadoBitCoin.GetBitCoinTrade: TTrades;
begin
  Result := Self.GetTrades( BitCoin,0,0 );
end;

function MercadoBitCoin.GetInfo: TInfo;

var trades:TTrades ;
    trade:TTrade;
    j:TJSONData;
    vstTexto:String;
    parser:TJSONParser;
    i:Integer;
    itemInfo:TInfoItem;
begin

  if not Assigned(FInfo) then
  begin

    vstTexto:= Self.GetRequestTApi( taGetInfo );
    try

      try
        parser := TJSONParser.Create(vstTexto);

        j := parser.Parse;

        If Assigned(J) then
        begin

          FInfo := Tinfo.Create;

          if j.FindPath('success').AsString = '1' then
          begin
            FInfo.success := True;

            FInfo.FReturn := TCollection.Create(TInfoItem);

            itemInfo := TInfoItem(FInfo.Freturn.Add);

            itemInfo.FFund := TFund.Create;

            itemInfo.FFund.brl := j.FindPath('return').FindPath('funds').FindPath('brl').AsFloat;
            itemInfo.FFund.btc := j.FindPath('return').FindPath('funds').FindPath('btc').AsFloat;
            itemInfo.FFund.ltc := j.FindPath('return').FindPath('funds').FindPath('ltc').AsFloat;
            itemInfo.FopenOrders := j.FindPath('return').FindPath('open_orders').AsInteger;
            itemInfo.FserverTime := j.FindPath('return').FindPath('server_time').AsInt64;

          end
          else
          begin
            FInfo.success:= false;
            FInfo.error:= j.FindPath('error').AsString;
          end;


        end;

      except
        on E:Exception do
          Application.MessageBox(PChar('Falha no Parser'+E.MEssage),'Teste',0);
      end;

    finally
      j.Free;
      parser.Free;
    end;

  end;
  Result := FInfo;
end;

function MercadoBitCoin.GetLiteCoinActiveOrderList: TOrderList;
var trades:TTrades ;
    trade:TTrade;
    j:TJSONData;
    vstTexto:String;
    parser:TJSONParser;
    i:Integer;
    itemInfo:TItemOrderList;
begin

  if not Assigned(FLiteCoinActiveOrderList) then
  begin

    vstTexto:= Self.GetRequestTApi( taOrderList );
    try

      try
        parser := TJSONParser.Create(vstTexto);

        j := parser.Parse;

        If Assigned(J) then
        begin

          FLiteCoinActiveOrderList := TOrderList.Create;

          if j.FindPath('success').AsString = '1' then
          begin
            FLiteCoinActiveOrderList.success := True;

            FLiteCoinActiveOrderList.FReturn := TCollection.Create(TItemOrderList);

            for i:=0 to j.FindPath('return').Count-1 do
            begin
              itemInfo := TItemOrderList(FLiteCoinActiveOrderList.Freturn.Add);

//              itemInfo.FIdOrder := j.FindPath('return').Items[i].AsString;

               ShowMessage(j.FindPath('return').AsJSON);




              itemInfo.FOrderListInfo := TItemOrderListInfo.Create;

              itemInfo.orderListInfo.status := solActive;
              itemInfo.orderListInfo.pair := pLtc_Brl;
              itemInfo.orderListInfo.created := j.FindPath('return').Items[i].FindPath('created').AsInt64;
              itemInfo.orderListInfo.price := j.FindPath('return').Items[i].FindPath('price').AsFloat;
              itemInfo.orderListInfo.volume := j.FindPath('return').Items[i].FindPath('volume').AsFloat;
            end;


          end
          else
          begin
            FInfo.success:= false;
            FInfo.error:= j.FindPath('error').AsString;
          end;


        end;

      except
        on E:Exception do
          Application.MessageBox(PChar('Falha no Parser'+E.MEssage),'Teste',0);
      end;

    finally
      j.Free;
      parser.Free;
    end;

  end;

  Result := FLiteCoinActiveOrderList;

end;

function MercadoBitCoin.GetLiteCoinTrade: TTrades;
begin
  Result := Self.GetTrades(LiteCoin,0,0);
end;

function MercadoBitCoin.GetTrades(CryptoCoin: TCryptoCoin; timeStampStart: Int64; timeStampFinal: Int64): TTrades;
var trades:TTrades ;
    trade:TTrade;
    j:TJSONData;
    vstTexto:String;
    parser:TJSONParser;
    i:Integer;
begin

  vstTexto:= Self.GetRequestApi( CryptoCoin, apTrade );
  try

    try
      parser := TJSONParser.Create(vstTexto);

      j := parser.Parse;

      If Assigned(J) then
      begin
        trades := TTrades.Create(TTrade);



        for i:=0 to J.Count-1 do
        begin
          trade := TTrade(trades.Add);
          trade.date := J.Items[i].Items[0].AsInt64;
          trade.price := J.Items[i].Items[1].AsFloat;
          trade.amount := J.Items[i].Items[2].AsFloat;
          trade.tid := J.Items[i].Items[3].AsInteger;

         { if lowerCase(J.Items[i].Items[4].AsString) = 'sell' then
            TTrade(trades.Add).typeTarde := Sell
          else
            TTrade(trades.Add).typeTarde := Buy;}
        end;

        Result := trades;
      end;

    except
      on E:Exception do
        Application.MessageBox(PChar('Falha no Parser'+E.MEssage),'Teste',0);
    end;

  finally
    j.Free;
    parser.Free;
  end;

end;

function MercadoBitCoin.GetTrades(CryptoCoin: TCryptoCoin): TTrades;
begin
  Result := Self.GetTrades(CryptoCoin, 0, 0);
end;

function MercadoBitCoin.MethodApi2String(method: TMethodApi): String;
begin

  case method of
    apTicker: Result := 'ticker';
    apOrderBook: Result := 'orderbook';
    apTrade: Result := 'trades';
  end;
end;

function MercadoBitCoin.MethodTradeApi2String(method: TMethodTApi): String;
begin
  case method of
    taGetInfo: Result := 'getInfo';
    taOrderList: Result := 'OrderList';
    taTrade: Result := 'Trade';
    taCancelOrder: Result := 'CancelOrder';
  end;
end;

{ Ticker }

function TTicker.GetBuy: Real;
begin
  Result := FBuy;
end;

function TTicker.GetHigh: Real;
begin
  Result := FHigh;
end;

function TTicker.GetLast: Real;
begin
  Result := FLast;
end;

function TTicker.GetLow: Real;
begin
  Result := FLow;
end;

function TTicker.GetSell: Real;
begin
  Result := FSell;
end;

function TTicker.GetTimeStamp: Int64;
begin
  Result := FTimeStamp;
end;

function TTicker.GetVolume: Real;
begin
  Result := FVolume;
end;

procedure TTicker.SetBuy(const Value: Real);
begin
  FBuy := Value;
end;

procedure TTicker.SetHigh(const Value: Real);
begin
  FHigh := Value;
end;

procedure TTicker.SetLast(const Value: Real);
begin
  FLast := Value;
end;

procedure TTicker.SetLow(const Value: Real);
begin
  FLow := Value;
end;

procedure TTicker.SetSell(const Value: Real);
begin
  FSell := Value;
end;

procedure TTicker.SetTimeStamp(const Value: Int64);
begin
  FTimeStamp := Value;
end;

procedure TTicker.SetVolume(const Value: Real);
begin
  FVolume := Value;
end;


{ OrderBook }

function OrderBook.GetAsk(Index: Integer): Order;
begin
  Result := FAsks[Index];
end;

function OrderBook.GetAskCount: Integer;
begin
  Result := Length(FAsks);
end;

function OrderBook.GetBid(Index: Integer): Order;
begin
  Result := FBids[Index];
end;

function OrderBook.GetBidsCount: Integer;
begin
  Result := Length(FBids);
end;

end.
