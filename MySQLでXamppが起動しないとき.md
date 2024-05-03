# MySQLでXamppが起動しないとき

1. XAMPPがインストールされているフォルダ（例：C:\xampp）が開くため、その中からmysqlフォルダを開く
1. dataフォルダをold_dataにリネーム
1. backupフォルダをコピー＆ペイストし、複製を作る（「backup - コピー」という名前になる）
1. 複製したbackupフォルダ（backup - コピー）をdataにリネーム
1. olda_dataフォルダ内のデータベース名と同じ名前のフォルダ（mysql、performance_shema、phpmyadmin以外）をdataフォルダにコピー
1. old_dataフォルダ内の「ibdata1」をdataフォルダに上書きコピー
1. XAMPPのコントロールパネルからMySQLを起動する

