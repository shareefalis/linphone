############################################################################
# CMakeLists.txt
# Copyright (C) 2014  Belledonne Communications, Grenoble France
#
############################################################################
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#
############################################################################

set(UI_FILES
	about.ui
	audio_assistant.ui
	buddylookup.ui
	call_logs.ui
	call_statistics.ui
	config-uri.ui
	contact.ui
	dscp_settings.ui
	keypad.ui
	ldap.ui
	log.ui
	main.ui
	parameters.ui
	password.ui
	provisioning-fetch.ui
	sip_account.ui
	tunnel_config.ui
	waiting.ui
)

set(PIXMAPS stock_people.png)
set(LICENSE ../COPYING)

set(SOURCE_FILES
	audio_assistant.c
	buddylookup.c
	calllogs.c
	chat.c
	conference.c
	config-fetching.c
	friendlist.c
	incall_view.c
	logging.c
	loginframe.c
	main.c
	propertybox.c
	singleinstance.c
	status_icon.c
	status_notifier.c
	support.c
	update.c
	utils.c
	videowindow.c
)
if(ENABLE_ASSISTANT)
	list(APPEND SOURCE_FILES setupwizard.c)
endif()
if(WIN32)
	list(APPEND SOURCE_FILES linphone.rc)
endif()

if(WIN32)
	add_executable(linphone-gtk WIN32 ${SOURCE_FILES})
else()
	add_executable(linphone-gtk ${SOURCE_FILES})
endif()
set_target_properties(linphone-gtk PROPERTIES OUTPUT_NAME linphone LINKER_LANGUAGE CXX)
target_include_directories(linphone-gtk PUBLIC ${GTK2_INCLUDE_DIRS} ${INTL_INCLUDE_DIRS})
target_link_libraries(linphone-gtk linphone ${GTK2_LIBRARIES})
if(INTL_FOUND)
	target_link_libraries(linphone-gtk ${INTL_LIBRARIES})
endif()

if(WIN32)
	target_link_libraries(linphone-gtk Wininet)
endif()
if(ENABLE_NOTIFY)
	target_include_directories(linphone-gtk PUBLIC ${NOTIFY_INCLUDE_DIRS})
	target_link_libraries(linphone-gtk ${NOTIFY_LIBRARIES})
endif()

install(TARGETS linphone-gtk
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
	PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
)

install(FILES ${UI_FILES} ${PIXMAPS} ${LICENSE}
	DESTINATION ${PACKAGE_DATA_DIR}/linphone
	PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
)
