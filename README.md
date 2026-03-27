# Relatório Técnico: Projeto de Integração Corporativa (Lab Virtual)

<p align="center">
  <img src="https://img.shields.io/badge/Status-Concluído-success?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/OS-Windows%2011-blue?style=for-the-badge&logo=windows" alt="Windows 11">
  <img src="https://img.shields.io/badge/OS-Debian%2013-red?style=for-the-badge&logo=debian" alt="Debian">
  <img src="https://img.shields.io/badge/Service-Samba-orange?style=for-the-badge" alt="Samba">
</p>

---

## 👤 Informações do Analista
* **Responsável:** Eduardo Nicolete
* **Tecnologias Utilizadas:** Windows 11, Debian 13 (Trixie), Samba, Hypervisor.

---

## 🚀 Fase 1: Preparação do Laboratório (Virtualização)
O objetivo inicial foi a implantação de duas Máquinas Virtuais (VMs) para simular o ambiente corporativo.

> [!IMPORTANT]
> **Desafios de Instalação:** Durante a implantação do Windows 11, foi encontrada uma barreira técnica devido às exigências modernas de segurança (TPM 2.0 e Secure Boot) impostas pelo sistema operacional.

**Solução Aplicada:** O problema foi mitigado através da reconfiguração da VM no Hypervisor, habilitando o suporte a **UEFI** e ativando o recurso **PAE/NX** nas configurações de processamento. Após esses ajustes, o sistema foi instalado com sucesso.

### Registro Visual da Instalação

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/91804e79-6fbf-409d-83d3-4c9ad95b8d43" alt="Tela de erro de instalação do Windows 11 por falta de TPM" style="max-width:100%;"><br>
      <em>Figura 1: Download de ambas máquinas ocorrendo de forma perfeita</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/25323cfa-f2f1-4cc1-8241-261c5396081a" alt="Configuração de processador na VM para habilitar PAE/NX" style="max-width:100%;"><br>
      <em>Figura 2: Bloqueio inicial na instalação do Windows 11 (requisitos de segurança).</em>
    </td>
  </tr>
</table>

---

## 🌐 Fase 2: Diagnóstico e Conectividade
Ajustes realizados para garantir que o servidor (Debian) e a estação de trabalho (Windows) se comunicassem corretamente na rede.

1.  **Configuração de Rede:** A placa de rede da VM Debian foi alterada do modo **NAT** para **Bridge**, permitindo que ambos os sistemas operassem no mesmo segmento de rede e recebessem IPs na mesma sub-rede.
2.  **Identificação de IP:**
    * **Windows 11:** Identificado via comando `ipconfig`. (IP: `10.0.2.15`)
    * **Debian 13:** Identificado via comando `ip a`. (IP: `10.87.38.21`)
3.  **Teste de Conectividade:** Foi realizado um teste de `ping`, confirmando a comunicação bidirecional bem-sucedida entre as máquinas.

---

## 🔒 Fase 3: Estrutura e Segurança (No Debian)
Seguindo as diretrizes de segurança da **InovaTech**, estruturei os diretórios e defini as permissões de acesso no servidor Debian.

* **Criação de Diretórios:** Utilizei o comando `mkdir` para criar a estrutura base em `/home/Compartilhamento/`.
* **Gestão de Permissões (`chmod`):**
    * A pasta `Publico` recebeu permissões totais (leitura e escrita) para todos os usuários.
    * A pasta `Secreto` foi configurada com acesso exclusivo ao **Superusuário (root)**, garantindo a integridade e confidencialidade de dados sensíveis.

### Estrutura de Diretórios e Permissões

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/a7ab93d2-7882-4027-9469-87e2c29ce8cb" alt="Terminal Debian mostrando a estrutura de diretórios e permissões aplicadas com chmod" style="max-width:80%;"><br>
      <em>Figura 3: Verificação da árvore de diretórios e permissões aplicadas no Linux.</em>
    </td>
  </tr>
</table>

---

## 🛠️ Fase 4: A Ponte (Samba)
Para habilitar o compartilhamento de arquivos nativo entre sistemas distintos (Linux e Windows), configurei o serviço Samba.

1.  **Atualização de Repositórios:** Como o Debian 13 (Trixie) estava configurado para buscar pacotes em mídia física (CD-ROM), editei o arquivo `/etc/apt/sources.list`. Comentei a linha do CD-ROM (inserindo `#`) e adicionei os repositórios oficiais via HTTP para permitir o download do Samba.
2.  **Configuração do `smb.conf`:** Editei o arquivo de configuração principal do Samba para declarar os diretórios:
    * Defini que a pasta `Publico` permitiria acesso a convidados (`guest ok = yes`).
    * Defini que a pasta `Secreto` exigiria autenticação obrigatória.
3.  **Criação de Usuário:** Utilizei o comando `smbpasswd -a [usuario]` para criar as credenciais de acesso externo necessárias para o Windows.

---

## ✅ Fase 5: Homologação
A fase de validação final ocorreu no ambiente Windows 11 para confirmar o sucesso da integração.

* No Explorador de Arquivos, acessei o endereço da VM Debian: `\\10.87.38.21\`
* O sistema Windows solicitou as credenciais de rede configuradas anteriormente no Samba.
* **Teste Final de Escrita:** Após a autenticação, foi possível visualizar os diretórios e criar com sucesso o arquivo `teste_de_rede.txt` dentro da pasta pública, comprovando que a integração corporativa foi concluída com êxito.

### Validação no Ambiente Windows

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/4c53dc3d-6fd0-42ac-8489-4d18877f5520" alt="Explorador de Arquivos do Windows acessando o IP do Debian, mostrando as pastas compartilhadas e o arquivo de teste criado." style="max-width:80%;"><br>
      <em>Figura 4: Acesso bem-sucedido aos compartilhamentos Samba e teste de criação de arquivo no Windows 11.</em>
    </td>
  </tr>
</table>
