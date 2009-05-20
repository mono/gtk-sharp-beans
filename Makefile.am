UNSAFE = -unsafe
DEBUG = -debug

GTK_SRCS =

GTK_CUSTOMS =					\
	$(srcdir)/Gtk.Customs/Builder.custom

GLIB_SRCS =					\
	$(srcdir)/GLib.Sources/Priority.cs	\
	$(srcdir)/GLib.Sources/Sources.cs	\
	$(srcdir)/GLib.Sources/Timeout.cs

ASSEMBLIES =					\
	-pkg:glib-sharp-2.0

all: gtk-sharp-beans.dll

gtk-api.xml: $(srcdir)/gtk-api.raw $(srcdir)/Gtk.metadata
	@echo -e "\n*** Massaging the raw api into $@"
	cp $(srcdir)/gtk-api.raw gtk-api.xml
	chmod +w $(builddir)/gtk-api.xml
	gapi2-fixup --api=gtk-api.xml --metadata=$(srcdir)/Gtk.metadata

gtk_generated/Builder.cs: gtk-api.xml $(GTK_CUSTOMS)
	@echo -e "\n*** Generating C# code"
	gapi2-codegen $(GLIBSHARP_CFLAGS) --outdir=gtk_generated --customdir=$(srcdir) --generate gtk-api.xml

gtk-sharp-beans.dll: $(srcdir)/gtk_generated/Builder.cs $(SRCS)
	@echo -e "\n*** Building $@"
	$(CSC) -target:library -out:$@ $(UNSAFE) $(DEBUG) $(ASSEMBLIES) $(GTK_SRCS) $(GLIB_SRCS) $(srcdir)/gtk_generated/*.cs

CLEANFILES = 			\
	gtk-sharp-beans.dll 	\
	gtk-sharp-beans.dll.mdb \
	gtk-api.xml

EXTRA_DIST =			\
	$(GTK_SRCS)		\
	$(GTK_CUSTOMS)		\
	$(GLIB_SRCS)		\
	gtk-api.raw
