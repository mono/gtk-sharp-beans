UNSAFE = -unsafe
DEBUG = -debug


GTK_SRCS =					\
	$(srcdir)/Gtk.Sources/Dialog.cs		\
	$(srcdir)/Gtk.Sources/Global.cs		\
	$(srcdir)/Gtk.Sources/Image.cs		\
	$(srcdir)/Gtk.Sources/Widget.cs

GTK_CUSTOMS =					\
	$(srcdir)/Gtk.Customs/Builder.custom

GLIB_SRCS =					\
	$(srcdir)/GLib.Sources/Format.cs	\
	$(srcdir)/GLib.Sources/Priority.cs	\
	$(srcdir)/GLib.Sources/Shell.cs		\
	$(srcdir)/GLib.Sources/Sources.cs	\
	$(srcdir)/GLib.Sources/Timeout.cs

GDK_SRCS =					\
	$(srcdir)/Gdk.Sources/PixbufFormat.cs	\
	$(srcdir)/Gdk.Sources/PixbufLoader.cs	\
	$(srcdir)/Gdk.Sources/Drawable.cs

SRCS =						\
	$(srcdir)/AssemblyInfo.cs		\
	$(GLIB_SRCS)				\
	$(GTK_SRCS)				\
	$(GDK_SRCS)

ASSEMBLIES =					\
	-pkg:glib-sharp-2.0			\
	-pkg:gio-sharp-2.0			\
	-pkg:gtk-sharp-2.0

all: gtk-sharp-beans.dll

gtk-api.xml: $(srcdir)/gtk-api.raw $(srcdir)/Gtk.metadata
	@echo -e "\n*** Massaging the raw api into $@"
	cp $(srcdir)/gtk-api.raw gtk-api.xml
	chmod +w $(builddir)/gtk-api.xml
	gapi2-fixup --api=gtk-api.xml --metadata=$(srcdir)/Gtk.metadata

gtkbeans-api.xml: $(srcdir)/gtkbeans-api.raw $(srcdir)/GtkBeans.metadata
	@echo -e "\n*** Massaging the raw api into $@"
	cp $(srcdir)/gtkbeans-api.raw gtkbeans-api.xml
	chmod +w $(builddir)/gtkbeans-api.xml
	gapi2-fixup --api=gtkbeans-api.xml --metadata=$(srcdir)/GtkBeans.metadata

gtk_generated/Builder.cs: gtk-api.xml gtkbeans-api.xml $(GTK_CUSTOMS)
	@echo -e "\n*** Generating C# code"
	gapi2-codegen $(GLIBSHARP_CFLAGS) --outdir=gtk_generated --customdir=$(srcdir)/Gtk.Customs --generate gtkbeans-api.xml
	gapi2-codegen $(GLIBSHARP_CFLAGS) $(GTKSHARP_CFLAGS) $(GIOSHARP_CFLAGS) --outdir=gtk_generated --customdir=$(srcdir)/Gtk.Customs --generate gtk-api.xml

gtk-sharp-beans.dll: $(srcdir)/gtk_generated/Builder.cs $(SRCS)
	@echo -e "\n*** Building $@"
	$(CSC) -target:library -out:$@ $(UNSAFE) $(DEBUG) $(ASSEMBLIES) $(SRCS) $(srcdir)/gtk_generated/*.cs

install-data-local: gtk-sharp-beans.dll
	@echo -e "\n*** Installing $< into the GAC"
	@echo "$(GACUTIL) /i $< /f $(GACUTIL_FLAGS)"
	@$(GACUTIL) /i $< /f $(GACUTIL_FLAGS) || exit 1;
	@echo

uninstall-local:
	@echo -e "\n*** Un-installing gio-sharp.dll from the GAC"
	@echo "$(GACUTIL) /u gtk-sharp-beans $(GACUTIL_FLAGS)"
	@$(GACUTIL) /u gtk-sharp-beans $(GACUTIL_FLAGS) || exit 1;
	@echo

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = gtk-sharp-beans-2.0.pc

CLEANFILES = 			\
	gtk-sharp-beans.dll 	\
	gtk-sharp-beans.dll.mdb \
	gtk-api.xml

EXTRA_DIST =			\
	$(SRCS)			\
	$(GTK_CUSTOMS)		\
	gtk-api.raw
