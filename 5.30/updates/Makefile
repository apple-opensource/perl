# Makefile updating perl
# Install into the Extras directory (just like CPAN), and after post-install
# cleanups, merge into the regular perl directories.

# add directories here
PROJECTS =

installarchlib := $(shell perl -MConfig -e 'print $$Config::Config{archlib}')
installbin := $(shell perl -MConfig -e 'print $$Config::Config{installbin}')
installprivlib := $(shell perl -MConfig -e 'print $$Config::Config{privlib}')
ARCHLIB := $(DSTROOT)/$(installarchlib)
PRIVLIB := $(DSTROOT)/$(installprivlib)
PLARGS := INSTALLDIRS=perl INSTALLARCHLIB='$(installarchlib)' INSTALLPRIVLIB='$(installprivlib)' INSTALLBIN='$(installbin)' INSTALLSCRIPT='$(installbin)'
PLBARGS := --installdirs core --install_path arch='$(installarchlib)' --install_path lib='$(installprivlib)' --install_path bin='$(installbin)' --install_path script='$(installbin)'
MAKEARGS := DESTDIR=$(DSTROOT) CONFIGDEP=
BUILDARGS := --destdir $(DSTROOT) CONFIGDEP=
export PERL5LIB := $(PRIVLIB)
export NO_PERL_PREPENDTOPATH := 1

no_target:
	@set -x && for i in $(PROJECTS); do \
	    echo "===== $$i =====" && \
	    $(MAKE) -C Modules/$$i unpack installarchlib="$(installarchlib)" \
		installprivlib="$(installprivlib)" PLARGS="$(PLARGS)" \
		PLBARGS="$(PLBARGS)" BUILDARGS="$(BUILDARGS)"\
		MAKEARGS="$(MAKEARGS)" && \
	    $(MAKE) -C Modules/$$i installarchlib="$(installarchlib)" \
		installprivlib="$(installprivlib)" PLARGS="$(PLARGS)" \
		PLBARGS="$(PLBARGS)" BUILDARGS="$(BUILDARGS)"\
		MAKEARGS="$(MAKEARGS)" || exit 1; \
	done

install:
	@set -x && for i in $(PROJECTS); do \
	    echo "===== $$i =====" && \
	    $(MAKE) -C Modules/$$i unpack installarchlib="$(installarchlib)" \
		installprivlib="$(installprivlib)" PLARGS="$(PLARGS)" \
		PLBARGS="$(PLBARGS)" BUILDARGS="$(BUILDARGS)"\
		MAKEARGS="$(MAKEARGS)" && \
	    $(MAKE) -C Modules/$$i install installarchlib="$(installarchlib)" \
		installprivlib="$(installprivlib)" PLARGS="$(PLARGS)" \
		PLBARGS="$(PLBARGS)" BUILDARGS="$(BUILDARGS)"\
		MAKEARGS="$(MAKEARGS)" || exit 1; \
	done
	@echo ================ post-install fixups ================
	rm -f $(ARCHLIB)/perllocal.pod
	find $(ARCHLIB)/auto -name .packlist -print -delete

projects:
	@echo $(PROJECTS)
