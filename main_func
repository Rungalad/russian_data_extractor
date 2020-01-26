# example string
string_with_dates = """Февраль 2017 г. – по октябрь 2017г c 31.02.2018 по 1.05.2015 и весь <02.1994>  но и 02-2018 и по н/вр
"""# и по н/вр
all_format_dmy = "[ ][0123]?\d{1}[.][01]\d{1}[.]\d{4}|[01]\d{1}[.]\d{4}[ г.,-]"

short_all_format_dmy = '[ ][0123]?\d{1}[.][01]\d{1}[.]\d{4}|[01]\d{1}[.]\d{2}[ г.,-]'

mnths = "[ ][Яя]нвар\w|[ ][Фф]еврал\w|[ ][Мм]арт\w*|[ ][Аа]прел\w|[ ][Мм]а[яй]|[ ][Ии]ю[нл][ьяе]|[ ][Аа]вгуст\w{0,1}|[ ][Сс]ентябр\w|[ ][Оо]ктябр\w|[ ][Нн]оябр\w|[ ][Дд]екабр\w"

years = "[21][09]\d{2}[ г.]" #?г?[.]?" #[г\- ]

for_today = 'н[.]вр[.]|н[\/]вр|н[\/]вр[.]|настоящее время|н[.]в[.]|[Pp]resent|[…]|по сегодняшний день|данный момент'

DAY_exp = '[ ][0123]?\d{1}[. ]?'

def ToDateFormat(TEXT, all_reg_dmy =  all_format_dmy, mnt_reg = mnths, years_reg = years, textual = for_today,
                DAY = False, day_exp = DAY_exp):
    text_ = ' ' + copy.deepcopy(TEXT) + ' '
    
    def OneTypeData(text_ = text_, REG = all_reg_dmy):
        regexdt = re.compile(REG)
        all_dmy = re.findall(regexdt, text_)
        all_dmy_ = []
        for i in all_dmy:
            i_s = text_.index(i)
            i_e = i_s + len(i)
            text_ = text_[:i_s] + '.'*len(i) + text_[i_e:] # Вырезаем найденное чтобы не повторяться
            all_dmy_ = all_dmy_ + [[i.replace('-', ''), i_s, i_e]]
        return (text_, all_dmy_)
    
    all_dmy_ = OneTypeData(text_ = text_, REG = all_reg_dmy) # Полное представление даты
    all_dmy_short = OneTypeData(text_ = all_dmy_[0], REG = short_all_format_dmy) # Сокращенное представление года в дате
    mnt_pol = OneTypeData(text_ = all_dmy_short[0], REG = mnt_reg) # месяцы
    year_pol = OneTypeData(text_ = mnt_pol[0], REG = years_reg) # годы тодельно
    YP = [[i[0].replace('г', '').replace('.', '').strip(), i[1], i[2]] for i in year_pol[1]]
    nv_pol = OneTypeData(text_ = year_pol[0], REG = textual) # текстовые представления нашего времени
    #print(nv_pol[0])
    if  DAY:
        day_pol = OneTypeData(text_ = nv_pol[0], REG = day_exp)
        DP = [[i[0].replace('г', '').replace('.', '').strip(), i[1], i[2]] for i in day_pol[1]]
        res = sorted([[i[1], i[0]] for i in all_dmy_[1] + all_dmy_short[1] + mnt_pol[1] + YP + nv_pol[1] + DP])
        return res, day_pol[0] # nv_pol[0] - это текст с вырезанными датами
    else:
        res = sorted([[i[1], i[0]] for i in all_dmy_[1] + all_dmy_short[1] + mnt_pol[1] + YP + nv_pol[1]])
        return res, nv_pol[0] # nv_pol[0] - это текст с вырезанными датами
    #print(nv_pol[0])

# example
ToDateFormat(string_with_dates, all_reg_dmy = all_format_dmy, mnt_reg = mnths, years_reg = years, DAY = False)

#result
#([[0, 'Февраль'],
#  [8, '2017 г.'],
#  [21, 'октябрь'],
#  [29, '2017г'],
#  [37, '31.02.2018'],
# [51, '1.05.2015'],
#  [69, '02.1994'],
#  [84, '02-2018'],
#  [97, 'н/вр']],
# '....... ....... – по ....... ..... c .......... по ......... и весь <.......>  но и ....... и по ....\n')
