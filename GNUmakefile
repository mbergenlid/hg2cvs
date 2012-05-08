HG2CVS_ROOT= $(shell pwd)
HG_REPO    = $(HG2CVS_ROOT)/hgrepo
CVS_REPO   = $(HG2CVS_ROOT)/cvsrepo
CVSROOT    = $(HG2CVS_ROOT)/cvsrepo/cvsroot

export CVSROOT

.PHONY: prepare
prepare: hgrepo.tar.bz2 purge
	mkdir $(HG_REPO) $(CVS_REPO)
	tar xf hgrepo.tar.bz2 -C $(HG_REPO)
	echo "[hooks]"                                             > $(HG_REPO)/gate/.hg/hgrc
	echo "changegroup = $(HG2CVS_ROOT)/hg2cvs.sh $(CVS_REPO)" >> $(HG_REPO)/gate/.hg/hgrc
	$(MAKE) clean

.PHONY: purge
purge:
	rm -rf $(HG_REPO) $(CVS_REPO)

.PHONY: clean
clean:
	rm -rf $(CVS_REPO)/* $(HG_REPO)/gate/.hg/hg2cvs*
	hg -R $(HG_REPO)/gate strip --no-backup 0
	hg clone $(HG_REPO)/gate $(CVS_REPO)/default
	cvs -d $(CVS_REPO)/cvsroot init
	cvs -d $(CVS_REPO)/cvsroot checkout -d $(CVS_REPO)/default .

.PHONY: run
run: $(CVS_REPO)/default $(CVS_REPO)/cvsroot
	hg -R $(HG_REPO)/source push $(HG_REPO)/gate