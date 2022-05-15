# Monitoração de Infraestrutura com Metricbeat
Olá pessoal. 

Vou trazer aqui alguns itens sobre a monitoração de infraestrutura com os componentes da Elastic. 

O desenvolvimento desse material foi feito com base em conhecimento das soluções nas versões 7.7.1 e 7.15.2. A ideia aqui é trazer visão sucinta dessas soluções. 



Imagine você poder centralizar seus dados de infraestrutura dentro de uma plantaforma que suporta outras frentes de sua empresa, como Logs de aplicações e APM. SIM! Isso felizmente é possível com os componentes do ELK Stack, é até uma curiosidade é que o ELK se passou a chamar ELK Stack devido os componentes que possibilitam monitorações de tal natureza. 



## O que uso para monitorar os recursos dos meus hosts?

A Elastic tem o componente Metricbeat que uma vez instalado no host permite a coleta de métricas. Utilizo com muito frequência a coleta de métricas de recursos básicos como memória, CPU, armazenamento. E em alguns casos métricas de serviços e processos do SO. 

Metricbeat, possui módulos especializados para soluções de mercado como Apache Kafka, MongoDB, Redis e diversos outros. 

O que você precisa saber aqui é que caso tenha a necessidade de monitoração de algum recurso ou software de um servidor (host) virtual, físico ou em cloud, provavelmente o Metricbeat irá lhe atender. 

Para verificar os módulos existentetes acessem o link  [Módulos](https://www.elastic.co/guide/en/beats/metricbeat/7.7/metricbeat-modules.html)([https://www.elastic.co/guide/en/beats/metricbeat/7.7/metricbeat-modules.html]) .





### Como o Metricbeat funciona?

O Metricbeat nada mais é do que um software que roda do lado do host e nele tem regras para coleta de métricas. Tal coleta ocorre periodicamente e é enviado para o Elasticsearch, para o Elasticsearch receberá nada mais e nada menos do que documentos JSON, cada campo desses documentos representam uma informação do host, para saber sobre o significado desses campos basta acessar a documentação oficial [Campos Metricbeat]([Exported fields | Metricbeat Reference [7.7\] | Elastic](https://www.elastic.co/guide/en/beats/metricbeat/7.7/exported-fields.html)).

As configurações relacionadas ao Metricbeat são realizadas atráves do arquivo metricbeat.yml, para cada módulo as configurações são localizadas /modules.d/NomeModule.yml  (exemplo /modules.d/windows.yml).

#### Bônus

A periodicidade de coleta da coleta dos dados estão relacionadas diretamente a armazenamento que será ocupado no Elasticseearch. Por padrão as métricas são coletadas com periodicidade alta, tive casos onde que pude subir a periodicidade para 3 vezes o valor padrão. Isso sem impactar a entrega de valor. Exemplo: o padrão de coleta para CPU e Memória é a cada 30 segundos, essa foi configuração foi alterada para 90 segundos. Sempre que forem pensar em uma decisão dessas, deve se levar em consideração se para o negócio ter os dados com atualizações mais espaçadas atendem a demanda da monitoração.

#### Metricbeat, coleta de serviço do Windows. 

Abaixo um exemplo da configuração de coleta do Windows, configurando a coleta para apenas os serviços que tem o Display name iniciando com 'Integration.App.Service'. Para tal configuração é necessário a alteração do nome do arquivo  /modules.d/windows.yml.disabled para /modules.d/windows.yml.

![image-20220515171340540](C:\Users\Altamir Dias Cassian\AppData\Roaming\Typora\typora-user-images\image-20220515171340540.png)

![image-20220515171402305](C:\Users\Altamir Dias Cassian\AppData\Roaming\Typora\typora-user-images\image-20220515171402305.png)
