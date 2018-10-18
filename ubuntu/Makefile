debug := -vvv
role := 

.PHONY: play
play:
	ansible-playbook play.yml -e 'role=${role}' ${debug}

.PHONY: all
all: 
	ansible-playbook all.yml ${debug}
