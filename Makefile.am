osname=$(subst ganeti-instance-,,$(PACKAGE))

osdir=$(OS_DIR)/$(osname)
customdir=${sysconfdir}/ganeti/instance-image/hooks
custom2dir=${sysconfdir}/ganeti/instance-image/pre-hooks
configdir=${sysconfdir}/ganeti/instance-image
variantsdir=${sysconfdir}/ganeti/instance-image/variants
networksdir=${sysconfdir}/ganeti/instance-image/networks
overlaysdir=${sysconfdir}/ganeti/instance-image/overlays
toolsdir=$(OS_DIR)/$(osname)/tools

dist_os_SCRIPTS = ${srcdir}/create ${srcdir}/import ${srcdir}/export \
	${srcdir}/rename ${srcdir}/verify
dist_os_DATA = ${srcdir}/ganeti_api_version
dist_config_DATA = ${srcdir}/variants.list ${srcdir}/parameters.list 
os_DATA = common.sh
tools_SCRIPTS = tools/apply-image tools/chroot tools/expand \
	tools/gnt-disk-tool tools/make-image tools/mount-disks tools/noop \
	tools/shell tools/show tools/shrink tools/umount-disks

dist_custom_DATA = ${srcdir}/example/hooks/*
dist_custom2_DATA = ${srcdir}/example/pre-hooks/*

dist_doc_DATA = ${srcdir}/COPYING ${srcdir}/README.markdown

EXTRA_DIST = common.sh.in defaults tools/apply-image.in tools/chroot.in \
	tools/expand.in tools/gnt-disk-tool.in tools/make-image.in \
	tools/mount-disks.in tools/noop.in tools/shell.in tools/show.in \
	tools/shrink.in tools/umount-disks.in ganeti-instance-image.spec \
	debian

edit = sed \
	   -e 's|@sysconfdir[@]|$(sysconfdir)|g' \
	   -e 's|@localstatedir[@]|$(localstatedir)|g' \
	   -e 's|@osdir[@]|$(OS_DIR)|g' \
	   -e 's|@osname[@]|$(osname)|g' \
	   -e 's|@defaultdir[@]|$(DEFAULT_DIR)|g' \
	   -e 's|@AWK[@]|$(AWK)|g' \
	   -e 's|@MKDIR_P[@]|$(MKDIR_P)|g' \
	   -e 's|@DUMP[@]|$(DUMP)|g' \
	   -e 's|@LOSETUP[@]|$(LOSETUP)|g' \
	   -e 's|@KPARTX[@]|$(KPARTX)|g' \
	   -e 's|@SFDISK[@]|$(SFDISK)|g' \
	   -e 's|@PARTED[@]|$(PARTED)|g' \
	   -e 's|@QEMU_IMG[@]|$(QEMU_IMG)|g' \
	   -e 's|@WGET[@]|$(WGET)|g' \
	   -e 's|@SHA1SUM[@]|$(SHA1SUM)|g' \
	   -e 's|@BLOCKDEV_CMD[@]|$(BLOCKDEV_CMD)|g' \
	   -e 's|@BLKID[@]|$(BLKID)|g'

common.sh: Makefile
	rm -f $@ $@.tmp
	srcdir=''; \
		   test -f ./$@.in || srcdir=$(srcdir)/; \
		   $(edit) $${srcdir}$@.in >$@.tmp
	mv $@.tmp $@

$(tools_SCRIPTS): Makefile
	@mkdir_p@ tools
	rm -f $@ $@.tmp
	srcdir=''; \
		   test -f ./$@.in || srcdir=$(srcdir)/; \
		   $(edit) $${srcdir}$@.in >$@.tmp
	mv $@.tmp $@

common.sh: $(srcdir)/common.sh.in
tools/apply-image: $(srcdir)/tools/apply-image.in
tools/chroot: $(srcdir)/tools/chroot.in
tools/expand: $(srcdir)/tools/expand.in
tools/gnt-disk-tool: $(srcdir)/tools/gnt-disk-tool.in
tools/make-image: $(srcdir)/tools/make-image.in
tools/mount-disks: $(srcdir)/tools/mount-disks.in
tools/noop: $(srcdir)/tools/noop.in
tools/shell: $(srcdir)/tools/shell.in
tools/show: $(srcdir)/tools/show.in
tools/shrink: $(srcdir)/tools/shrink.in
tools/umount-disks: $(srcdir)/tools/umount-disks.in

install-exec-local:
	@mkdir_p@ "$(DESTDIR)$(OS_DIR)/$(osname)"
	@mkdir_p@ "$(DESTDIR)$(variantsdir)"
	@mkdir_p@ "$(DESTDIR)$(networksdir)/subnets"
	@mkdir_p@ "$(DESTDIR)$(networksdir)/instances"
	@mkdir_p@ "$(DESTDIR)$(overlaysdir)"
	@mkdir_p@ "$(DESTDIR)$(localstatedir)/cache/ganeti-instance-image"
	touch "$(DESTDIR)$(variantsdir)/default.conf"
	ln -fs $(DESTDIR)$(configdir)/variants.list $(DESTDIR)$(OS_DIR)/$(osname)/variants.list

distclean-local:
	rm -rf build-stamp debian/files debian/*.log debian/ganeti-instance-image/ \
		debian/ganeti-instance-image.substvars

CLEANFILES = $(os_DATA) $(tools_SCRIPTS)
