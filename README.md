# 👨‍💻 LAB Splunk

## 🐳 Docker

```
$ docker pull splunk/splunk:latest

$ docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "SPLUNK_PASSWORD=<password>" --name splunk splunk/splunk:latest

Login: admin
```

1. ### Inicia um container do Docker desanexado usando a `splunk/splunk:latest`imagem.

2. ### Exponha um mapeamento de porta do host `8000`para o container `8000`.

3. ### Especifique um personalizado `SPLUNK_PASSWORD`- certifique-se de substituir `<password>`
   
   - **DEVE** ser string que esteja em conformidade com os [requisitos de senha](https://docs.splunk.com/Documentation/Splunk/latest/Security/Configurepasswordsinspecfile).
   
4. ### Aceite o contrato de licença com `SPLUNK_START_ARGS=--accept-license`. Isso deve ser explicitamente aceito em todos os `splunk/splunk`contêineres, caso contrário, o Splunk não iniciará.

---

## ⛏ Query

```
index=_introspection host="*" status="success"
| stats latest(timestamp) as start by host 
| join host 
    [ search index=_introspection status="failure" host=* 
    | stats latest(timestamp) as finish by host ] 
| eval difference=finish-start
| table difference
```

### O recurso ☝ possibilita realização de consultas que medem a diferença de um atributo, resultado de duas consultas diferentes possibilitando a extração da duração de processos definidos.   
