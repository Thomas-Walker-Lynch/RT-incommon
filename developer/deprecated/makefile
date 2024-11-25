SHELL=/usr/bin/bash

.PHONEY: install
install:
	sudo pwd
	cd iseq_C && make install
	cd iseq_interpreted; sudo install -m 755 * /usr/local/bin

