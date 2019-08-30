### url:wss://ws.hologo.io/kline-api/ws(��)

## ����-K������

* ����:
```
{"event":"sub","params":{"channel":"market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]","cb_id":"�Զ���"}}
```
* ���ض���״̬1��:
```
{"event_rep":"subed","channel":"market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]","cb_id":"ԭ·����","ts":1506584998239,"status":"ok"}
```
* �������ض�����Ϣ:
```
{
    "channel":"market_$base$quote_kline_[1min/5min/15min/30min/60min/1day/1week/1month]",//���ĵĽ��׶�����$base$quote��ʾbtckrw��
    "ts":1506584998239,//����ʱ��
    "tick":{
        "id":1506602880,//ʱ��̶���ʼֵ
        "amount":123.1221,//���׶�
        "vol":1212.12211,//������
        "open":2233.22,//���̼�
        "close":1221.11,//���̼�
        "high":22322.22,//��߼�
        "low":2321.22//��ͼ�
    }
}
```