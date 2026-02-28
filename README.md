# Backup-Autom-tico-de-Arquivos-M-dio-
Preenchimento: configure ORIGEM e DESTINO nas linhas 14-15

"""
Backup Automático de Pastas
Preenchimento: configure ORIGEM e DESTINO nas linhas 14-15
"""
import shutil
import os
from datetime import datetime
from pathlib import Path

class BackupAutomático:
    def __init__(self):
        # ← PREENCHA AQUI: pasta que quer fazer backup
        self.pasta_origem = r"C:\Users\SeuUsuario\Documentos"
        # ← PREENCHA AQUI: onde salvar o backup (HD externo, nuvem, etc)
        self.pasta_destino = r"D:\Backup"
        
        self.timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        self.nome_backup = f"backup_{self.timestamp}"
    
    def executar(self):
        """Executa backup completo"""
        try:
            origem = Path(self.pasta_origem)
            destino = Path(self.pasta_destino) / self.nome_backup
            
            if not origem.exists():
                print(f"❌ Pasta de origem não encontrada: {origem}")
                return
            
            destino.mkdir(parents=True, exist_ok=True)
            
            # Copia arquivos
            arquivos_copiados = 0
            for item in origem.rglob("*"):
                if item.is_file():
                    rel_path = item.relative_to(origem)
                    dest_file = destino / rel_path
                    dest_file.parent.mkdir(parents=True, exist_ok=True)
                    shutil.copy2(item, dest_file)
                    arquivos_copiados += 1
            
            print(f"✅ Backup concluído!")
            print(f"📁 Arquivos copiados: {arquivos_copiados}")
            print(f"📍 Local: {destino}")
            
        except Exception as e:
            print(f"❌ Erro no backup: {e}")

# === COMO CONFIGURAR ===
# 1. Altere 'pasta_origem' para a pasta que deseja backup
# 2. Altere 'pasta_destino' para onde quer guardar (HD externo, Dropbox, etc)
# 3. Execute: python backup.py
# 4. Ou agende no Cron/Agendador de Tarefas para automação

if __name__ == "__main__":
    print("💾 Iniciando Backup...")
    backup = BackupAutomático()
    backup.executar()
