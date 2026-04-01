# Relatório Técnico: Projeto de Integração Corporativa (Lab Virtual)

<p align="center">
  <img src="https://img.shields.io/badge/Status-Em%20Análise-yellow?style=for-the-badge" alt="Status">
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
O objetivo inicial foi a implantação de duas Máquinas Virtuais (VMs).

> [!IMPORTANT]
> **Desafios de Instalação:** Durante a instalação do Windows 11, foi encontrada uma barreira técnica devido às exigências de segurança modernas (TPM 2.0 e Secure Boot).

**Solução Aplicada:** O problema foi mitigado através da configuração da VM para suporte a **UEFI** e a habilitação do recurso **PAE/NX** nas configurações de processamento do Hypervisor.

### Registro Visual da Instalação

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/91804e79-6fbf-409d-83d3-4c9ad95b8d43" alt="Download das VMs"><br>
      <em>Figura 1: Download das imagens ISO e preparação do ambiente.</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/25323cfa-f2f1-4cc1-8241-261c5396081a" alt="Bloqueio Windows 11"><br>
      <em>Figura 2: Bloqueio de instalação por requisitos de hardware.</em>
    </td>
  </tr>
</table>

---

## 🌐 Fase 2: Diagnóstico e Conectividade
Ajustes realizados para garantir a comunicação entre o servidor e a estação de trabalho:

1. **Configuração de Rede:** A placa de rede do Debian foi alterada de **NAT** para **Bridge**.
2. **Identificação de IP:**
   * **Windows 11:** `10.0.2.15` (ipconfig).
   * **Debian 13:** `10.87.38.21` (ip a).
3. **Teste de Conectividade:** Foi realizado o teste de `ping`, confirmando a comunicação bidirecional.

### Comandos de Rede

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/47a7af49-fcbf-421c-be16-46e3d520804e" alt="IP Debian"><br>
      <em>Figura 3: Verificação de interface no Debian.</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/007c8d30-0740-4f7b-8736-229210a779a3" alt="IP Windows"><br>
      <em>Figura 4: Verificação de interface no Windows.</em>
    </td>
  </tr>
</table>

---

## 🔒 Fase 3: Estrutura e Segurança (No Debian)
Estruturação dos diretórios seguindo as diretrizes da InovaTech:

* **Diretórios:** Criação da estrutura em `/home/Compartilhamento/`.
* **Permissões:**
    * **Publico:** Definida para leitura/escrita global via `chmod`.
    * **Secreto:** Acesso restrito ao **root**, visando proteger dados sensíveis.

### Configuração de Diretórios

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/b1b71001-b252-4ea3-be88-fd11616e6e02" alt="mkdir debian"><br>
      <em>Figura 5: Criação das pastas via terminal.</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/791ab27d-bd94-454d-830a-9d0d15c8f18f" alt="chmod debian"><br>
      <em>Figura 6: Ajuste de permissões.</em>
    </td>
  </tr>
  <tr>
    <td align="center" colspan="2">
      <img src="https://github.com/user-attachments/assets/a7ab93d2-7882-4027-9469-87e2c29ce8cb" alt="Estrutura de diretórios"><br>
      <em>Figura 7: Validação da estrutura de pastas.</em>
    </td>
  </tr>
</table>

---

## 🛠️ Fase 4: A Ponte (Samba)
Configuração do serviço para compartilhamento entre Linux e Windows:

1. **Repositórios:** Edição do `/etc/apt/sources.list` para desativar o CD-ROM e habilitar espelhos oficiais.
2. **Instalação:** Download e configuração do pacote Samba no Debian 13.
3. **Parametrização e Usuário:** Edição do arquivo de configuração e criação de credenciais via `smbpasswd`.

### Implementação do Serviço

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/1a8987a6-18cf-4932-9316-217df0673028" alt="Configuração CD-ROM"><br>
      <em>Figura 8: Ajuste nas sources do APT.</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/f2ab5776-3576-4c8c-867d-6163470c1547" alt="smbpasswd configuration"><br>
      <em>Figura 9: Configuração do Samba (smb.conf).</em>
    </td>
  </tr>
  <tr>
    <td align="center" colspan="2">
      <img src="https://github.com/user-attachments/assets/b322e9fa-e5f4-4878-b57c-7c29308e5503" alt="Instalação Samba"><br>
      <em>Figura 10: Processo de atualização e instalação do serviço.</em>
    </td>
  </tr>
</table>

---

## ❌ Fase 5: Homologação e Problemas Encontrados
A tentativa de validação foi realizada acessando `\\10.87.38.21\` pelo Explorador de Arquivos do Windows 11.

> [!WARNING]
> **Ponto Crítico:** Apesar das configurações realizadas, não foi possível concluir a escrita de arquivos de forma funcional. O sistema impediu a criação de novos arquivos na pasta pública, e o problema não pôde ser corrigido durante o período de laboratório.

### Evidências do Acesso

<table width="100%">
  <tr>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/2ab67da3-bd92-4eec-b176-8efd1ada7c2c" alt="Acesso Windows"><br>
      <em>Figura 11: Visualização das pastas compartilhadas no Windows.</em>
    </td>
    <td align="center" width="50%">
      <img src="https://github.com/user-attachments/assets/5302d6bd-1dc7-4241-aadf-a283092b530f" alt="Erro escrita"><br>
      <em>Figura 12: Erro de permissão persistente ao tentar criar arquivo na pasta pública.</em>
    </td>
  </tr>
</table>
