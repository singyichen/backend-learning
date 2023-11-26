---
title: Fastify Dependencies ( WebSocket )
description: WebSocket 套件
published: true
date: 2023-11-21T05:04:15.614Z
tags: fastify, framework
editor: markdown
dateCreated: 2023-08-16T07:37:07.904Z
---

# WebSocket 套件介紹 ( WebSocket Dependencies )
- [ ] [How to test Socket.io with Jest on backend (Node.js)?](https://medium.com/@tozwierz/testing-socket-io-with-jest-on-backend-node-js-f71f7ec7010f)

## socket.io
> - Socket.io 是一個現成的 WebSocket 套件
> - Socket.io 支持基於事件的實時雙向通信

> [reference](https://socket.io/)
{.is-info}

### Install
```shell
npm i socket.io --save
```
- 單元測試使用
```shell
npm i socket.io-client --save-dev
```
> [reference](https://www.npmjs.com/package/socket.io)
{.is-info}

### Usage
- 使用情境：實作逾期自動登出功能

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI1NjFweCIgaGVpZ2h0PSIxMTAxcHgiIHZpZXdCb3g9Ii0wLjUgLTAuNSA1NjEgMTEwMSIgY29udGVudD0iJmx0O214ZmlsZSBob3N0PSZxdW90O2VtYmVkLmRpYWdyYW1zLm5ldCZxdW90OyBtb2RpZmllZD0mcXVvdDsyMDIzLTA4LTE4VDA5OjE2OjQ3LjExMVomcXVvdDsgYWdlbnQ9JnF1b3Q7TW96aWxsYS81LjAgKFdpbmRvd3MgTlQgMTAuMDsgV2luNjQ7IHg2NCkgQXBwbGVXZWJLaXQvNTM3LjM2IChLSFRNTCwgbGlrZSBHZWNrbykgQ2hyb21lLzExNi4wLjAuMCBTYWZhcmkvNTM3LjM2JnF1b3Q7IGV0YWc9JnF1b3Q7dUVqemJrdjNJaXo2TXVyR25Kb24mcXVvdDsgdmVyc2lvbj0mcXVvdDsyMS42LjgmcXVvdDsgdHlwZT0mcXVvdDtlbWJlZCZxdW90OyZndDsmbHQ7ZGlhZ3JhbSBpZD0mcXVvdDs2aWN1N2V2cjF0djlud2FxN1p2ZCZxdW90OyBuYW1lPSZxdW90O1BhZ2UtMSZxdW90OyZndDs3VnBiYzVzNEdQMDFldXdPTjRIMENEYnB6azQ3MDVuc1ROdEhETExOQmlNdnhyRzl2MzRsSVJrRVNrb2FZNmVaNXNYb2t3VG9uTzl5SkFMYzJlYjRzVXEyNjg4MEl3VndyT3dJM0Rsd0hOdHpQZmJETGFmR0VtQ25NYXlxUEpPRFdzTjkvaCtSUmt0YTkzbEdkdHJBbXRLaXpyZTZNYVZsU2RKYXN5VlZSUS82c0NVdDlLZHVreFVaR083VHBCaGF2K1padlc2c3lBbGErNThrWDYzVmsyMGZOejJiUkEyV0s5bXRrNHdlT2lZM0J1NnNvclJ1cmpiSEdTazRlQXFYWnQ3ZEU3M25GNnRJV1krWklIRi9USXE5WEJ1SU1VQVdpR0lRK3dEUEFMNERNUUlvQUdFSVlnaFFCREFFY1FBd0JsRWtMQUdJUWo0NFl0YzI3d29SSDlhc3J6NHAwSGFIZkZNa0pXdEY2VG92c2svSmllNzVTKzdxSkgxUXJhZ2lPOGIzRjdVQ3UyZjZuSEF2c3BpVlRhdHE2UnVRRzVhMFZHM0haKzBoRmhLZVIxTFY1Tmd4U1d3K0Vyb2hkWFZpUTFTdkwzbVNqZ3BsODlDeUR0V1F0Y2E0SmEySmRMWFYrZDR0Ryt4Q0VtSW14eldRd3pCbS9Nd0Z6QkVJNzU2RHVZdVExMFBJUnJ5ZEY4V01GclFTVTkwc0lXaVppb2tWZlNDZEhqOUZaTEhrUGZMdWQvck1lUmlqdTVrUmMyYzA1azlqN0NEZmdMRUZMNEJ4TU1UWThZdGFncVZCNi8rN3A2cmp3MDdBR0xJQnRyYzlpcFdyZm5hMUVyOWFsRUFRUW5YclJYVWVJeTNzTlp2bktYT1AxWXJ1eTR4azB2VVA2N3dtOTlzazViMEhsbU9aYlYxdkNoa3ZmWnFIcExpalNaRzlnUjRId1pBajJ6UEVnWDhCaGlDZWtpS1I3RUtYNXk4VUM0cXNIVTBmaUxndnFSaENCdEpZUWd4WjFrT0s0Vm1EWmI0OC9mWDFiM1lOWW8vblJKWUUrVVVFSWwrNGdBTWlWNVNxQjFLK0cxYzRsN0pyK0FLYTFCY0dSVzBVODM2eTRhQ1hpeDMvS2VpSzFiRlprUk5CNFZpT0cvTnVtNVF2V29PMVlLVnpKVHppUTlxa1k5NmZsM21kSjRWNW1iMFhOdmhxKzVMTkMvMGFqb2l3ZlQxSFZNL3F3RUV5cGhGbGsxYjFtamxDbVJSeGE0MTB3Tm94bnlqZFNwaitJWFY5a2pnbCs1cnFJREtzcXRNM1B2OFBCNnIyZDNrLzBaZ2Z0ZFpKdHA1RWUwZjNWU29YNE1wU3pWVERpdFI2d1BIRlBjdEpSWXFremg5MWdmd3E4V09RcHBjTGRTUkVLaElhRjdKNGRpMFI5Y3ptODVxQUxZQVJ0ekFaakdaaWxLZXlPQUtSMWFrYkxCdXdWQkRxc2huelpNOHRNeTdSK05OQ0VIcnFzVUl0TTZuTTFkdXZXZ1ZzcUFXZnIzWnczZUJ6cGdvKzU4YkJGMHdRZk82YkNqN0QxdU9TZFpadFhWQlRaK2RjRS8yT0wwTjgyYjBBTTFTM3FRTE1HN0xmNXNJcE5wNlFvTXd6YlR3dDhmZk14aFBHYU82OWN1TjU3TzB5bjltSUJ0NFE4Y3RzUkZWQ3ZkSk9sQVZQeU9wY0lHSm16ZzhWZU13Z1h2bjRtSkNmK3hqa0x3UTRBamgrdTlzWjc2V3M5MDU0YkFQcms2bElCeGtDemVmU2cyYzF5TWtJL1hGVTlWRmQwODFpdjNzWm90NHdNcGZMcFpNYWo0UXlmK0ZELzl5anprR2RTVmlCN2todGp5N0JpdUZNNk14S0gyaTJubHBIVkp4V0pnc3hnTHMwVnhJN2lUQnZGdm1xWk5jcEE0Y3dMQ01PU3A0bVJTZzdObm1XQ2NXeXBYbFppM1hBQ01CNWo1eVN5bFRiNVVVYTlaanFVM3dCZWxROVUvUjR3NkF4c2VOZTRsRFVjQVJ3RHBYZjdJaGU5WDFGYVFmN2V2UmdhMERDZGJYNUs2VzVSSzRyelpITUIxMXByakwzOWFVNW12ckVPaEt5RzNFQkhjM0JEdzgyRFNKaFVLOWFrYUFmUGIwWDBlRFo3dlZFQTU1NGJ6WndnTjZSNXNqVFE5MGhtRGZnbWJqd2hZQVpiTy9lajJ2b3VSZjcxenlWREc2UmZJMlo4SWRaRmNOaFZtMXkyeFdTS0o1d3N6WGk4MUVUQnMxMkNvb3Y3Rzg2SGdiT2J5QjNiS284QzRScnhJTTlWSVJ2V0l6MGxTQTU1dlUzZFU5Mi9iMjlKV3UxTitHTnB3V05PaXZ0aEo1Qno2alRoK3ZyR2Y4bWduRVVUclloUmNHYm5jbDY3aTJBZXNvcHJSYzQ1ZVhVdVlFa3BjUzdKS2x2UnpjZzZTWVpaMERTVHdKK0p2ZE02UGRPL25sRnhqR1M1RjZhSkRuMUM5K1p0d1hJeFZpclFMNGI2TGRvWGtyTzZsRjlmbzF4c3NLN0JmdVRocGY2QnRWbERsK2N1ZEc2N1RZRi9TY0s4ZXRJZVZaTUcxaUNKcGJnTktTd1p2di9xVTJRdFAvbDY4Yi9Bdz09Jmx0Oy9kaWFncmFtJmd0OyZsdDsvbXhmaWxlJmd0OyI+PGRlZnMvPjxnPjxwYXRoIGQ9Ik0gMCA1MCBMIDAgMCBMIDU2MCAwIEwgNTYwIDUwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cGF0aCBkPSJNIDAgNTAgTCAwIDExMDAgTCA1NjAgMTEwMCBMIDU2MCA1MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDAgNTAgTCA1NjAgNTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PGcgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXdlaWdodD0iYm9sZCIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMjZweCI+PHRleHQgeD0iMjc5LjUiIHk9IjM2LjUiPumAvuacn+iHquWLleeZu+WHuua1geeoizwvdGV4dD48L2c+PHBhdGggZD0iTSAwIDkwIEwgMCA1MCBMIDI4NiA1MCBMIDI4NiA5MCIgZmlsbD0iI2RhZThmYyIgc3Ryb2tlPSIjNmM4ZWJmIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PHBhdGggZD0iTSAwIDkwIEwgMCAxMTAwIEwgMjg2IDExMDAgTCAyODYgOTAiIGZpbGw9IiNkYWU4ZmMiIHN0cm9rZT0iIzZjOGViZiIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMCA5MCBMIDI4NiA5MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjNmM4ZWJmIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PGcgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXdlaWdodD0iYm9sZCIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMThweCI+PHRleHQgeD0iMTQyLjUiIHk9Ijc3LjUiPuWJjeerrzwvdGV4dD48L2c+PHJlY3QgeD0iNzAiIHk9IjEyMCIgd2lkdGg9IjE0MCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTUwcHg7IG1hcmdpbi1sZWZ0OiA3MXB4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMThweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IG5vbmU7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE0cHg7Ij7nmbvlhaU8YnIgLz48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjE0MCIgeT0iMTU1IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMThweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+55m75YWlJiN4YTs8L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjcwIiB5PSIyNzAiIHdpZHRoPSIxNDAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDEzOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDMwMHB4OyBtYXJnaW4tbGVmdDogNzFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE4cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyI+6YCj5o6lIHNvY2tldCBzZXJ2ZXI8YnIgLz7op7jnmbwgdmVyaWZ5SldUIOS6i+S7tuWCsyB0b2tlbjxiciAvPjwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTQwIiB5PSIzMDUiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxOHB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7pgKPmjqUgc29ja2V0IHNlcnZlci4uLjwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iNzAiIHk9Ijk0MSIgd2lkdGg9IjE0MCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogOTcxcHg7IG1hcmdpbi1sZWZ0OiA3MXB4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMThweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IG5vbmU7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE0cHg7Ij7nmbvlh7o8YnIgLz7op7jnmbzCoGxvZ291dENsaWVudDxiciAvPjwvZm9udD48c3BhbiBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyBiYWNrZ3JvdW5kLWNvbG9yOiBpbml0aWFsOyI+wqDkuovku7Y8L3NwYW4+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjE0MCIgeT0iOTc2IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMThweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+55m75Ye66Ke455m8wqBsb2dvdXRDbGllbnQuLi48L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNzUgODAyIEwgNzUgODcxLjUgTCAxMDUgODcxLjUgTCAxMDUgOTM0LjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMTA1IDkzOS44OCBMIDEwMS41IDkzMi44OCBMIDEwNSA5MzQuNjMgTCAxMDguNSA5MzIuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxyZWN0IHg9IjE1IiB5PSI3NDIiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDc3MnB4OyBtYXJnaW4tbGVmdDogMTZweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE4cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyI+6KiI5pW4MzDliIbpkJjlvozmlLbliLDpgKPnt5rpgL7mmYLpjK/oqqToqIrmga88YnIgLz48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9Ijc1IiB5PSI3NzciIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxOHB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7oqIjmlbgzMOWIhumQmOW+jOaUtuWIsOmAo+e3mumAvuaZgumMr+iqpOioiuaBryYjeGE7PC90ZXh0Pjwvc3dpdGNoPjwvZz48cGF0aCBkPSJNIDIxMSA4MDEgTCAyMTEgODcxIEwgMTc1IDg3MSBMIDE3NSA5MzQuNjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PHBhdGggZD0iTSAxNzUgOTM5Ljg4IEwgMTcxLjUgOTMyLjg4IEwgMTc1IDkzNC42MyBMIDE3OC41IDkzMi44OCBaIiBmaWxsPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PHJlY3QgeD0iMTUxIiB5PSI3NDEiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIGZpbGw9InJnYigyNTUsIDI1NSwgMjU1KSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDc3MXB4OyBtYXJnaW4tbGVmdDogMTUycHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxOHB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogbm9uZTsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZTogMTRweDsiPueri+WNs+aUtuWIsOmAo+e3mumAvuaZgumMr+iqpOioiuaBrzxiciAvPjwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMjExIiB5PSI3NzYiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxOHB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7nq4vljbPmlLbliLDpgKPnt5rpgL7mmYLpjK/oqqToqIrmga8mI3hhOzwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSAyODYgOTAgTCAyODYgNTAgTCA1NjAgNTAgTCA1NjAgOTAiIGZpbGw9IiNkNWU4ZDQiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMjg2IDkwIEwgMjg2IDExMDAgTCA1NjAgMTEwMCBMIDU2MCA5MCIgZmlsbD0iI2Q1ZThkNCIgc3Ryb2tlPSIjMDAwMDAwIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PHBhdGggZD0iTSAyODYgOTAgTCA1NjAgOTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzAwMDAwMCIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxnIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC13ZWlnaHQ9ImJvbGQiIHBvaW50ZXItZXZlbnRzPSJub25lIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjE4cHgiPjx0ZXh0IHg9IjQyMi41IiB5PSI3Ny41Ij7lvoznq688L3RleHQ+PC9nPjxyZWN0IHg9IjM0NiIgeT0iMjAwIiB3aWR0aD0iMTQwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyMzBweDsgbWFyZ2luLWxlZnQ6IDM0N3B4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMThweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IG5vbmU7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE0cHg7Ij7nmbvlhaXpqZforYnmiJDlip88YnIgLz7lm57lgrMgdG9rZW48YnIgLz48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQxNiIgeT0iMjM1IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMThweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+55m75YWl6amX6K2J5oiQ5Yqf5Zue5YKzIHRva2VuLi4uPC90ZXh0Pjwvc3dpdGNoPjwvZz48cGF0aCBkPSJNIDQxNiA1ODEgTCA0ODYgNjIxIEwgNDE2IDY2MSBMIDM0NiA2MjEgWiIgZmlsbD0iI2ZmZjJjYyIgc3Ryb2tlPSIjZDZiNjU2IiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogNjIxcHg7IG1hcmdpbi1sZWZ0OiAzNDdweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE0cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij7mmK/lkKbpqZforYnmiJDlip88L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNDE2IiB5PSI2MjUiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxNHB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7mmK/lkKbpqZforYnmiJDlip88L3RleHQ+PC9zd2l0Y2g+PC9nPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxcHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogNjA1cHg7IG1hcmdpbi1sZWZ0OiAzMjlweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE0cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm93cmFwOyI+5pivPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjMyOSIgeT0iNjA5IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTRweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+5pivPC90ZXh0Pjwvc3dpdGNoPjwvZz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMXB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDY3NXB4OyBtYXJnaW4tbGVmdDogNDQwcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxNHB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogbm9uZTsgd2hpdGUtc3BhY2U6IG5vd3JhcDsiPuWQpjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0NDAiIHk9IjY3OSIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjE0cHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuWQpjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0MTYgNTIzIEwgNDE2IDU3NC42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDQxNiA1NzkuODggTCA0MTIuNSA1NzIuODggTCA0MTYgNTc0LjYzIEwgNDE5LjUgNTcyLjg4IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cmVjdCB4PSIzNDYiIHk9IjQ2MyIgd2lkdGg9IjE0MCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogNDkzcHg7IG1hcmdpbi1sZWZ0OiAzNDdweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE4cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyI+55uj6IG9IHZlcmlmeUpXVCDkuovku7Y8YnIgLz7pqZforYkgdG9rZW7CoDxiciAvPjwvZm9udD48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNDE2IiB5PSI0OTgiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxOHB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7nm6Pogb0gdmVyaWZ5SldUIOS6i+S7ti4uLjwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMzUwIiB5PSIxMDExIiB3aWR0aD0iMTQwIiBoZWlnaHQ9IjYwIiBmaWxsPSJyZ2IoMjU1LCAyNTUsIDI1NSkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMDQxcHg7IG1hcmdpbi1sZWZ0OiAzNTFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE4cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyI+55uj6IG9IGxvZ291dENsaWVudMKg5LqL5Lu2PGJyIC8+6Zec6ZaJ6YCj57eawqA8YnIgLz48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQyMCIgeT0iMTA0NiIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjE4cHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuebo+iBvSBsb2dvdXRDbGllbnTCoOS6i+S7ti4uLjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0MTYgNDAwIEwgNDE2IDQ1Ni42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDQxNiA0NjEuODggTCA0MTIuNSA0NTQuODggTCA0MTYgNDU2LjYzIEwgNDE5LjUgNDU0Ljg4IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cmVjdCB4PSIzNDYiIHk9IjM0MCIgd2lkdGg9IjE0MCIgaGVpZ2h0PSI2MCIgZmlsbD0icmdiKDI1NSwgMjU1LCAyNTUpIiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzcwcHg7IG1hcmdpbi1sZWZ0OiAzNDdweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDE4cHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBub25lOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNHB4OyI+c29ja2V0IHNlcnZlcjxiciAvPumWi+WVn+mAo+e3msKgPGJyIC8+PC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0MTYiIHk9IjM3NSIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjE4cHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPnNvY2tldCBzZXJ2ZXIuLi48L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gMjEwIDE1MCBMIDQxNiAxNTAgTCA0MTYgMTkzLjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gNDE2IDE5OC44OCBMIDQxMi41IDE5MS44OCBMIDQxNiAxOTMuNjMgTCA0MTkuNSAxOTEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMzQ2IDIzMCBMIDE0MCAyMzAgTCAxNDAgMjYzLjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMTQwIDI2OC44OCBMIDEzNi41IDI2MS44OCBMIDE0MCAyNjMuNjMgTCAxNDMuNSAyNjEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMzQ2IDYyMSBMIDc1IDYyMSBMIDc1IDczNS42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDc1IDc0MC44OCBMIDcxLjUgNzMzLjg4IEwgNzUgNzM1LjYzIEwgNzguNSA3MzMuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gNDE2IDY2MSBMIDQxNiA3MDEgTCAyMTEgNzAxIEwgMjExIDczNC42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDIxMSA3MzkuODggTCAyMDcuNSA3MzIuODggTCAyMTEgNzM0LjYzIEwgMjE0LjUgNzMyLjg4IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9Im5vbmUiLz48cGF0aCBkPSJNIDIxMCA5NzEgTCA0MjAgOTcxIEwgNDIwIDEwMDQuNjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJub25lIi8+PHBhdGggZD0iTSA0MjAgMTAwOS44OCBMIDQxNi41IDEwMDIuODggTCA0MjAgMTAwNC42MyBMIDQyMy41IDEwMDIuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gMjEwIDMwMCBMIDQxNiAzMDAgTCA0MTYgMzMzLjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxwYXRoIGQ9Ik0gNDE2IDMzOC44OCBMIDQxMi41IDMzMS44OCBMIDQxNiAzMzMuNjMgTCA0MTkuNSAzMzEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjwvZz48c3dpdGNoPjxnIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIvPjxhIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAsLTUpIiB4bGluazpocmVmPSJodHRwczovL3d3dy5kcmF3aW8uY29tL2RvYy9mYXEvc3ZnLWV4cG9ydC10ZXh0LXByb2JsZW1zIiB0YXJnZXQ9Il9ibGFuayI+PHRleHQgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMHB4IiB4PSI1MCUiIHk9IjEwMCUiPlRleHQgaXMgbm90IFNWRyAtIGNhbm5vdCBkaXNwbGF5PC90ZXh0PjwvYT48L3N3aXRjaD48L3N2Zz4=
```

#### 初始寫法
- 在 `src/plugin` 中新增一個 `socketIO.js`
- socket server 需與一般 restful server 拆分，故須使用**不同的 port 進行監聽**
- 此處的 socket server url 為 `http://127.0.0.1:8117/`
- 此處的 restful server url 為 `http://127.0.0.1:8116/`
- 新增一個事件監聽 `verifyJWT`：用來驗證 token 使用
- 新增一個事件監聽 `logoutClient`：用來關閉連線使用

```js
/**
 * @description socket io plugin
 */

const fastifyPlugin = require('fastify-plugin');
const http = require('http');
const socketIO = require('socket.io');
const VerificationService = require('../modules/admin/basic/service/verificationService');
const verificationService = new VerificationService();
const { ErrorResponse } = require('../base/base.response');
const { registerUserJwtExpiredInfo } = require('../utils/errorInfo');

async function socketIOConnector(fastify, opts, done) {
  // 將 fastify 放進 http 中開啟 Server 的 SOCKET_IO_PORT
  const server = http.Server(fastify);
  // 將啟動的 Server 送給 socket.io 處理
  const io = socketIO(server);

  // 監聽 Server 連線後的所有事件，並捕捉事件 socket 執行
  io.on('connection', function (socket) {
    // 連線成功
    console.log('socket io server connected');

    // 監聽透過 connection 傳進來的事件
    socket.on('verifyJWT', async (message) => {
      console.log('verifyJWT event trigger');
      const { token_id } = message;
      console.log('get token_id：' + token_id);
      const data = await verificationService.verifyJWT(token_id);
      // JWT 驗證成功
      if (data.code === '00') {
        console.log('verifyJWT token_id：' + token_id + ' success');
        // 計數(毫秒)
        setTimeout(function () {
          // 回傳 message 給發送訊息的 Client
          socket.emit(
            'verifyJWT',
            new ErrorResponse(registerUserJwtExpiredInfo),
          );
        }, +process.env.JWT_EXPIRESIN);
      } else {
        console.log('verifyJWT token_id：' + token_id + ' failed');
        // JWT 驗證失敗
        socket.emit('verifyJWT', new ErrorResponse(registerUserJwtExpiredInfo));
      }
    });
    socket.on('logoutClient', () => {
      // 連線關閉
      console.log('user logout and disconnected');
      socket.disconnect();
    });
    socket.on('disconnect', function (message) {
      console.log('socket io server disconnected');
    });
  });
  server.listen(
    { port: process.env.SOCKET_IO_PORT, host: '0.0.0.0' },
    function (err) {
      console.log(
        `Welcome to e_board_API socket io server listening at http://${process.env.IP_ADDRESS}:${process.env.SOCKET_IO_PORT}`,
      );
      if (err) {
        console.log(err);
      }
    },
  );

  done();
}

module.exports = fastifyPlugin(socketIOConnector);
```

- Postman 測試 socket.io api
- 連接會顯示 `Connected to http://127.0.0.1:8117/`
- 不連接會顯示 `Disconnected from http://127.0.0.1:8117/`

![Eboard System Postman socket io api connect.png](http://192.168.25.60:8000/files/file_storage/9196203c.png)

- 新增監聽事件：`verifyJWT`

![Eboard System Postman socket io api add event.png](http://192.168.25.60:8000/files/file_storage/4040d7de.png)

- 觸發監聽事件(需指定事件名稱)並傳送需驗證的 `token_id`，得到來自 socket io server 的回應

![Eboard System Postman socket io api send message via event.png](http://192.168.25.60:8000/files/file_storage/fdcceb4c.png)

- docker 部屬時須注意的地方：Dockerfile 需設定兩個 EXPOSE

```Dockerfile
# Stage 5: Set container listen Port
# 設定 Docker 要開放的 Port
# restful api server
EXPOSE 8116
# socket server
EXPOSE 8117
```

- `docker run` 指令也需設定兩個 port 的對應 `-p 8116:8116 -p 8117:8117`

```
`docker run -d --name e_board_api -p 8116:8116 -p 8117:8117 --restart=always -v /var/log/eboard:/src/logs -v /Volume/eboard/db/sqlite.db:/src/sqlite.db -v /Volume/eboard/.env:/src/.env -v /Volume/eboard/config:/src/config e_board_api:1.2.5`
```

#### Server 與 事件監聽拆分寫法，並配合進行單元測試
- 在 `src/plugin` 中新增一個 `socketIOServer.js`
- 新增跨網域設定
- import `socketIO` ( socket io api )
- 須分別 export `io` 與 `server`，以利進行單元測試
- 須分別 export `fastifyPlugin`，可於 `app.js` 中 import
- server 直接監聽 `port` 即可，不需要 `host` `'0.0.0.0'` 部屬到 docker 上仍可使用，若仍監聽 `host` 於單元測試時會有問題

```js
/**
 * @description socket io server plugin
 */

const fastify = require('fastify');
const fastifyPlugin = require('fastify-plugin');
const http = require('http');
// socket io api
const socketIO = require('./socketIOApi');
// 將 fastify 放進 http 中開啟 Server 的 SOCKET_IO_PORT
const server = http.Server(fastify);
// 將啟動的 Server 送給 socket.io 處理
const io = new socketIO(server, {
  cors: {
    origin: [
      process.env.BACKEND_IP || 'http://192.168.25.180',
      process.env.BACKEND_TEAM_LEADER_IP || 'http://192.168.25.140',
      process.env.FRONTEND_IP || 'http://192.168.25.100:3000',
      process.env.FRONTEND_PC_IP || 'http://192.168.240.197:3000',
      process.env.DOCKER01_SRV,
      process.env.DOCKER02_SRV,
      process.env.DOCKER03_SRV || 'http://192.168.25.60:8000',
      process.env.DOCKER03_ESERVICE || 'http://eservice.hokia.com.tw:8000',
    ],
  },
});
// 直接監聽 port 即可，不需要 host '0.0.0.0' 部屬到 docker 上仍可使用
server.listen(process.env.SOCKET_IO_PORT, function (err) {
  console.log(
    `Welcome to e_board_API socket io server listening at http://${process.env.IP_ADDRESS}:${process.env.SOCKET_IO_PORT}`,
  );
  if (err) {
    console.log(err);
  }
});
async function socketIOServerConnector(fastify, opts, done) {
  done();
}

module.exports.io = io;
module.exports.server = server;
module.exports.fastifyPlugin = fastifyPlugin(socketIOServerConnector);
```

- 調整 `app.js` 中的 `fastifySocketIOServerPlugin` 引用方式

```js
// app.js
const fastifySocketIOServerPlugin = require('./src/plugin/socketIOServer').fastifyPlugin;
```

- 在 `src/plugin` 中新增一個 `socketIOApi.js`
- 可於此檔引入各模組的 api 監聽
- 回傳 `io` 可於 `socketIOServer.js` import

```js
/**
 * @description socket io api plugin
 */

const socketIO = require('socket.io');
// import 各模組 api
const adminBasicSocket = require('../modules/admin/basic/socket/basicSocket');
const adminUserSocket = require('../modules/admin/user/socket/userSocket');

module.exports = function (server) {
  const io = socketIO(server);
  // 監聽 Server 連線後的所有事件，並捕捉事件 socket 執行
  io.on('connection', (socket) => {
    // 連線成功
    console.log('socket io server connected');

    // 測試案例中的監聽
    socket.on('echo', function (message) {
      socket.emit('echo', 'Hello World');
    });

    // 模組 api 監聽
    adminBasicSocket.basicSocket(socket);
    adminUserSocket.userSocket(socket);

    // 連線關閉
    socket.on('disconnect', function (message) {
      console.log('socket io server disconnected');
    });
  });

  return io;
};
```

- 新增各模組 api 監聽

```js 
// userSocket.js
const UserService = require('../service/userService');
const userService = new UserService();
const { apiAdminUserFindAllUrl } = require('../../../../utils/url');
/**
 * @description user socket
 */
async function userSocket(socket) {
  /**
   * @description 取得多筆人員
   * @returns { Object } json
   */
  try {
    socket.on(apiAdminUserFindAllUrl, async (message) => {
      console.log(`${apiAdminUserFindAllUrl} event trigger`);
      socket.emit(apiAdminUserFindAllUrl, await userService.findAll());
    });
  } catch (error) {
    console.error(`Socket.IO error：${apiAdminUserFindAllUrl}`, error);
  }
}
module.exports = { userSocket };
```

- 新增 `socketIOServer.js` 的單元測試 `socketIOServer.test.js`
- 測試從 server 到 client
- 測試從 client 到 server：server 中需有此測試監聽 event ，最後監聽 clientSocket ，因若有得到訊息表示是由 server 監聽到並回傳過來的

```js
// socketIOServer.test.js
const io = require('socket.io-client');
// server
const server = require('../../../src/plugin/socketIOServer').server;
const serverSocket = require('../../../src/plugin/socketIOServer').io;
let serverAddr;
// client
let clientSocket;
/**
 * @description unit test for socket io server plugin
 */
describe('Test socket io server plugin', () => {
  /**
   * Setup WS & HTTP servers
   */
  beforeAll((done) => {
    serverAddr = server.address();
    done();
  });

  /**
   *  Cleanup WS & HTTP servers
   */
  afterAll((done) => {
    serverSocket.close();
    server.close();
    done();
  });

  beforeEach((done) => {
    // Setup
    // Do not hardcode server port and address, square brackets are used for IPv6
    clientSocket = io.connect(
      `http://[${serverAddr.address}]:${serverAddr.port}`,
      {
        'reconnection delay': 0,
        'reopen delay': 0,
        'force new connection': true,
        transports: ['websocket'],
      },
    );
    clientSocket.on('connect', () => {
      done();
    });
  });

  afterEach((done) => {
    // Cleanup
    if (clientSocket.connected) {
      clientSocket.disconnect();
    }
    done();
  });
  /**
   * @description unit test for socket io server
   */
  describe(`Test socket io server plugin should have a socket io server`, () => {
    test(`Test socket io server should communicate when Server emit event echo with eventMessageFromServer Hello World, Client should listen event echo with eventMessageFromServer Hello World`, (done) => {
      // Arrange
      const event = 'echo';
      const eventMessageFromServer = 'Hello World';

      // Act
      serverSocket.emit(event, eventMessageFromServer);

      // Assert
      clientSocket.once(event, (message) => {
        expect(message).toBe(eventMessageFromServer);
        done();
      });
    });
    test(`Test socket io server should communicate when Client emit event echo with eventMessageFromClient Hello World, Client should listen event echo with eventMessageFromServer Hello World`, (done) => {
      // Arrange
      const event = 'echo';
      const eventMessageFromClient = 'Hello World';
      const eventMessageFromServer = 'Hello World';

      // Act
      clientSocket.emit(event, eventMessageFromClient);

      // Assert
      clientSocket.once(event, (message) => {
        expect(message).toBe(eventMessageFromServer);
        done();
      });
    });
  });
});

```

- 新增 `userSocket.js` 的單元測試 `userSocket.test.js`

```js
// userSocket.test.js
const io = require('socket.io-client');
// server
const server = require('../../../../../../src/plugin/socketIOServer').server;
const serverSocket = require('../../../../../../src/plugin/socketIOServer').io;
let serverAddr;
// client
let clientSocket;
// socket api
const { apiAdminUserFindAllUrl } = require('../../../../../../src/utils/url');
const { StatusCodes } = require('http-status-codes');
/**
 * @description unit test for UserSocket
 */
describe('Test UserSocket', () => {
  /**
   * Setup WS & HTTP servers
   */
  beforeAll((done) => {
    serverAddr = server.address();
    done();
  });

  /**
   *  Cleanup WS & HTTP servers
   */
  afterAll((done) => {
    serverSocket.close();
    server.close();
    done();
  });

  beforeEach((done) => {
    // Setup
    // Do not hardcode server port and address, square brackets are used for IPv6
    clientSocket = io.connect(
      `http://[${serverAddr.address}]:${serverAddr.port}`,
      {
        'reconnection delay': 0,
        'reopen delay': 0,
        'force new connection': true,
        transports: ['websocket'],
      },
    );
    clientSocket.on('connect', () => {
      done();
    });
  });

  afterEach((done) => {
    // Cleanup
    if (clientSocket.connected) {
      clientSocket.disconnect();
    }
    done();
  });
  /**
   * @description unit test for event apiAdminUserFindAllUrl
   */
  describe(`Test UserService should have a listen event ${apiAdminUserFindAllUrl}`, () => {
    test(`Test socket io server should communicate when Server emit event ${apiAdminUserFindAllUrl} with eventMessageFromServer Hello World, Client should listen event ${apiAdminUserFindAllUrl} with eventMessageFromServer Hello World`, (done) => {
      // Arrange
      const event = apiAdminUserFindAllUrl;
      const eventMessageFromServer = 'Hello World';

      // Act
      serverSocket.emit(event, eventMessageFromServer);

      // Assert
      clientSocket.once(event, (message) => {
        expect(message).toBe(eventMessageFromServer);
        done();
      });
    });
    test(`Test socket io server should communicate when Client emit event ${apiAdminUserFindAllUrl} with eventMessageFromClient "", Client should listen event ${apiAdminUserFindAllUrl} with a json include statusCode and data and dateTime`, (done) => {
      // Arrange
      const event = apiAdminUserFindAllUrl;
      const eventMessageFromClient = '';

      // Act
      clientSocket.emit(event, eventMessageFromClient);

      // Assert
      clientSocket.once(event, (message) => {
        expect(message.statusCode).toBe(StatusCodes.OK);
        expect(typeof message.data).toBe('object');
        expect(typeof message.dateTime).toBe('object');
        done();
      });
    });
  });
});

```





