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

includes = []
extra_libs = []
includes = include_directories('..')

executable('gnome-terminal', sources,
  dependencies: [
    libgtk,
    libgio,
    libgconf,
    libgsettings,
    libgthread,
    libvte
  ],
  link_with: extra_libs,
  include_directories: includes,
  c_args: cflags,
  install: true
)
