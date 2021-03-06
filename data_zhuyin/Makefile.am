## Makefile.am -- Process this file with automake to produce Makefile.in
## Copyright (C) 2011 Peng Wu
##
## This program is free software; you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation; either version 2, or (at your option)
## any later version.
##
## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with this program; if not, write to the Free Software
## Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.


tablefiles		= tsi.table

binfiles		= ${tablefiles:.table=.bin}


textual_model_data	= interpolation2.text \
				$(tablefiles)


binary_model_data	= phrase_index.bin pinyin_index.bin \
				bigram.db \
				$(binfiles)


MAINTAINERCLEANFILES	= Makefile.in

EXTRA_DIST		= $(textual_model_data) \
                          table.conf

libzhuyin_db_DATA 	= $(binary_model_data) \
                          table.conf

libzhuyin_dbdir		= $(libdir)/libzhuyin/data

CLEANFILES		= $(binary_model_data)

interpolation2.text:
	wget http://downloads.sourceforge.net/libzhuyin/models/model11.text.tar.gz
	tar xvf model11.text.tar.gz -C $(top_srcdir)/data_zhuyin


$(tablefiles) table.conf: interpolation2.text

bigram.db: $(textual_model_data)
	$(RM) $(binary_model_data)
	../utils/storage/gen_binary_files --table-dir $(top_srcdir)/data_zhuyin
	../utils/storage/import_interpolation --table-dir $(top_srcdir)/data_zhuyin < $(top_srcdir)/data_zhuyin/interpolation2.text
	../utils/training/gen_unigram --table-dir $(top_srcdir)/data_zhuyin

phrase_index.bin pinyin_index.bin $(binfiles): bigram.db

modify:
	git reset --hard
	sed -i -r -e "s'lambda parameter:0\\.[0-9]{3,6}'lambda parameter:$(LAMBDA_PARAMETER)'" table.conf
