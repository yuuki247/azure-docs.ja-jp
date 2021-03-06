* Azure Storage Service Encryption を使って暗号化されているストレージ アカウントに非管理対象ディスクが存在する場合、そのディスクを管理ディスクに変換することはできません。 これらの仮想ハード ディスク (VHD) をコピーして管理ディスクで使用する手順については、この記事の後半の「[Managed Disks と Azure Storage Service Encryption](#managed-disks-and-azure-storage-service-encryption)」を参照してください。

* 変換作業は VM の再起動を伴うので、既に設定されているメンテナンス期間中に VM の移行をスケジュールしてください。 

* 一度行った変換を元に戻すことはできません。 

* 変換のテストは必ず行ってください。 まずテスト用の仮想マシンで試してから運用環境で移行を実行します。

* 変換中に、VM の割り当てを解除します。 VM は、変換後に起動されたときに、新しい IP アドレスを受け取ります。 VM には必要に応じて[静的 IP アドレスを割り当て](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md)ることができます。

* 変換前の VM で使用されていた元の VHD とストレージ アカウントは削除されません。 その後も料金が発生します。 これらのアーティファクトに対して課金されないように、変換が完了したことを確認したら、元の VHD BLOB は削除してください。