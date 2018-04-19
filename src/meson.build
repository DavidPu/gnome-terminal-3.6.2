gnome = import('gnome')

sources = [
'eggdesktopfile.c',
'eggshell.c',
'eggsmclient.c',
'profile-editor.c',
'terminal-accels.c',
'terminal-app.c',
'terminal.c',
'terminal-close-button.c',
'terminal-debug.c',
'terminal-encoding.c',
'terminal-info-bar.c',
'terminal-marshal.c',
'terminal-options.c',
'terminal-profile.c',
'terminal-screen.c',
'terminal-screen-container.c',
'terminal-search-dialog.c',
'terminal-tab-label.c',
'terminal-tabs-menu.c',
'terminal-type-builtins.c',
'terminal-util.c',
'terminal-window.c'
]

libgtk = dependency('gtk+-3.0', version: '>= 3.18')
libgconf = dependency('gconf-2.0', version: '>= 2.0')
libglib = dependency('glib-2.0', version: '>= 2.0')
libvte = dependency('vte-2.90')
libgsettings = dependency('gsettings-desktop-schemas')
libgthread = dependency('gthread-2.0')
localedir = join_paths(get_option('prefix'), get_option('localedir'))
cflags = [
  '-DG_SETTINGS_ENABLE_BACKEND',
  '-DPACKAGE_LOCALEDIR="@0@"'.format(localedir),
  '-DVERSION="@0@"'.format(meson.project_version()),
  '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name()),
  '-DGLIB_VERSION_MIN_REQUIRED=GLIB_VERSION_2_44',
  '-DGLIB_VERSION_MAX_ALLOWED=GLIB_VERSION_2_44',
  '-DTERMINAL_COMPILATION',
  '-DEXECUTABLE_NAME="gnome-terminal"',
  '-DSN_API_NOT_YET_FROZEN',
  '-DGDK_MULTIHEAD_SAFE',
  '-DG_DISABLE_SINGLE_INCLUDES',
  '-DPANGO_DISABLE_SINGLE_INCLUDES',
  '-DGDK_DISABLE_SINGLE_INCLUDES',  
  '-DGDK_PIXBUF_DISABLE_SINGLE_INCLUDES',
  '-DGTK_DISABLE_SINGLE_INCLUDES',
]

if get_option('buildtype') == 'release'
  cflags += [
    '-DG_DISABLE_ASSERT=1',
    '-DG_DISABLE_CHECKS=1',
    '-DG_DISABLE_CAST_CHECKS=1'
  ]
endif

if libgtk.version().version_compare('>= 3.20')
  cflags += [
    '-DGDK_VERSION_MIN_REQUIRED=GDK_VERSION_3_20',
    '-DGDK_VERSION_MAX_ALLOWED=GDK_VERSION_3_20'
  ]
else
  cflags += [
    '-DGDK_VERSION_MIN_REQUIRED=GDK_VERSION_3_18',
    '-DGDK_VERSION_MAX_ALLOWED=GDK_VERSION_3_18'
  ]
endif

if target_machine.system() == 'windows'
  libgio = dependency('gio-2.0', version: '>= 2.44')
else
  libgio = dependency('gio-unix-2.0', version: '>= 2.44')
endif

#generated_gdbus_sources = gnome.gdbus_codegen(
#  'gmpv_mpris_gdbus',
#  '../data/gmpv_mpris_gdbus.xml',
#  namespace: 'gmpv_mpris'
#)

#generated_marshal_sources = gnome.genmarshal('gmpv_marshal',
#  sources: '../data/gmpv_marshal.lst',
#  prefix: 'g_cclosure_gen_marshal',
#)

# NOTE: All of these are fixed in GLib 2.54 and Meson 0.42.0 and can be
# removed once version requirements are bumped
includes = []
extra_libs = []
if libgio.version().version_compare('< 2.52.0')
  # We want to be warning free on old versions:
  # https://bugzilla.gnome.org/show_bug.cgi?id=778581
  extra_libs += static_library('mpris-gdbus',
    c_args: cflags + ['-Wno-conversion'],
    dependencies: libgio,
    include_directories: include_directories('..'),
  )
  # Created includes of `src/...`:
  # https://bugzilla.gnome.org/show_bug.cgi?id=778801
  includes = include_directories('..')
endif

includes = include_directories('..')
if libgio.version().version_compare('< 2.54.0')
  # Silence warning about missing prototypes in genmarshal-generated files
  extra_libs += static_library('marshal',
    c_args: cflags + ['-Wno-missing-prototypes'],
    dependencies: libgio,
    include_directories: include_directories('..')
  )
else
  sources += generated_marshal_sources
endif

executable('gnome-terminal', sources,
  dependencies: [
    libgtk,
    libgio,
    libgconf,
    libvte,
    libgsettings,
    libgthread
  ],
  link_with: extra_libs,
  include_directories: includes,
  c_args: cflags,
  install: true
)
