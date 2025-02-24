# Pré-requisitos

Para usar a integração do Mercado Pago com o Magento 2 em sua loja, você deve atender aos requisitos descritos na tabela abaixo.

> WARNING 
> 
> Mantenha seu plugin atualizado para não perder vendas
> 
> Em outubro de 2022, as versões do plugin Mercado Pago anteriores a 3.5.0 serão descontinuadas e deixarão de funcionar. Além disso, em abril as bandeiras de cartões de crédito aplicaram mudanças internacionais nas transações.  
>
> **Mantenha sua loja sempre atualizada com a versão mais recente.**

| Requisitos  | Descrição | 
| --- | --- |
| Versão do Magento | 2.x |
| Ambiente | LAMP (Linux, Apache, MySQL e PHP)<br/>Pilha LNMP |
| Sistema operacional | Linux x86-64 |
| Memória | Mínimo de 2 GB de RAM |
| Servidor Web | Apache 2.x<br/>Nginx 1.7.x |
| Versão PHP | 5.6.x<br/>7.0.2<br/>7.0.6–7.0.x<br/> |
| Versão MySQL | MySQL 5.6<br/>MariaDB e Percona são compatíveis com Magento porque suportam APIs MySQL 5.6. |
| Dependências de extensões | bc-math (apenas Magento Commerce)<br/>curl<br/>gd, ImageMagick 6.3.7 (ou posterior) ou ambos<br/>intl<br/>bstring<br/>mcrypt<br/>hash<br/>openssl<br/>PDO / MySQL<br/>SimpleXML<br/>sabão<br/>xml <br/>xsl<br/>zip<br/> |
| PHP 7 apenas | json<br/>iconv |
| SSL | É necessário ter um certificado SSL e que a forma de pagamento esteja disponibilizada em uma página HTTPS.<br/>Durante os testes em modo Sandbox, você poderá executar os testes em HTTP. |

> Este módulo está configurado para suportar os padrões do Magento 2. Recomendamos que você não utilize plugins ou módulos que alterem as características e operação do padrão do Magento para evitar possíveis erros no módulo ou que ele pare de funcionar.