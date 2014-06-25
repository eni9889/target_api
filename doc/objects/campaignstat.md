
## CampaignStat

Объект с полной информацией о кампании (совместно со статистикой)

<table>
    <thead>
        <tr><th>Параметр</th><th>Тип</th><th>Значение</th></tr>
    </thead>
    <tbody>
        <tr>
            <td><code>id</code></td>
            <td><code>Integer</code></td>
            <td><p><br />Идентификатор кампании</p></td>
        </tr><tr>
            <td><code>name</code></td>
            <td><code>String</code></td>
            <td><p><em>40 символов</em> <br />Название кампании</p></td>
        </tr><tr>
            <td><code>status</code></td>
            <td><code>String</code></td>
            <td><p><em>255 символов</em> <em>active, deleted, blocked</em><br />Статус кампании</p></td>
        </tr><tr>
            <td><code>system_status</code></td>
            <td><code>String</code></td>
            <td><p><em>255 символов</em> <em>active, deleted, blocked</em></p></td>
        </tr><tr>
            <td><code>created</code></td>
            <td><code>DateTime</code></td>
            <td><p><br />Время создания</p></td>
        </tr><tr>
            <td><code>updated</code></td>
            <td><code>DateTime</code></td>
            <td><p><br />Время последнего обновления</p></td>
        </tr><tr>
            <td><code>date_start</code></td>
            <td><code>Date</code></td>
            <td><p><br />Время старта кампании</p></td>
        </tr><tr>
            <td><code>date_end</code></td>
            <td><code>Date</code></td>
            <td><p><br />Время окончания кампании</p></td>
        </tr><tr>
            <td><code>package</code></td>
            <td><code>[Package](package)</code></td>
            <td><p><br />Структура пакета</p></td>
        </tr><tr>
            <td><code>price_per_show</code></td>
            <td><code>Decimal</code></td>
            <td><p><br />Цена за показ в рублях</p></td>
        </tr><tr>
            <td><code>price_per_click</code></td>
            <td><code>Decimal</code></td>
            <td><p><br />Цена за клик в рублях</p></td>
        </tr><tr>
            <td><code>budget_limit_day</code></td>
            <td><code>String</code></td>
            <td><p><em>32 символа</em> <br />Бюджет кампании на день</p></td>
        </tr><tr>
            <td><code>budget_limit</code></td>
            <td><code>String</code></td>
            <td><p><em>32 символа</em> <br />Общий бюджет кампании</p></td>
        </tr><tr>
            <td><code>mixing</code></td>
            <td><code>String</code></td>
            <td><p><em>64 символа</em> <em>fastest, recommended</em><br />Распределение бюджета</p></td>
        </tr><tr>
            <td><code>targetings</code></td>
            <td><code>String</code><code>CampaignTargetings</code>
```json
{
  "pads": "List, Рекламные площадки",
  "age": "IntegerList, Возраст",
  "regions": "IntegerList, Регионы",
  "regions_names": "List",
  "grouped_regions_names": "List",
  "sex": "String, Пол",
  "week_days": "List",
  "day_hours": "IntegerList",
  "education": "List, Образование",
  "salary": "List, Диапазоны заработных плат",
  "language": "Language, Языки",
  "profession": "List, Профессии",
  "paid": "String, История платежей",
  "birthday": "Birthday, День рождения",
  "tree": "IntegerList",
  "user_geo": "Dict, Географическое положение, указанное пользователем",
  "fulltime": "Fulltime, Время (дни и часы)",
  "remarketing": "List, Вхождение в аудитории",
  "current_game": "Boolean, Таргетинг на приложение (в Одноклассниках или Моём Мире), рекламируемого в объявлении кампании",
  "current_group": "Boolean, Таргетинг на группу (в Одноклассниках или Моём Мире), рекламируемую в объявлении кампании",
  "thematics": "IntegerList",
  "mobile_types": "List, Типы мобильных устройств",
  "mobile_operation_systems": "IntegerList, Мобильные операционные системы",
  "mobile_operators": "IntegerList, Мобильные операторы",
  "mobile_vendors": "IntegerList, Производители мобильных устройств",
  "interests": "IntegerList, Интересы пользователей",
  "binmask": "",
  "custom": ""
}
```
</td>
            <td><p><br />Структура таргетингов</p></td>
        </tr><tr>
            <td><code>url</code></td>
            <td><code>String</code></td>
            <td><p><em>1024 символа</em> </p></td>
        </tr><tr>
            <td><code>edit_url</code></td>
            <td><code>String</code></td>
            <td><p><em>1024 символа</em> </p></td>
        </tr><tr>
            <td><code>banners_url</code></td>
            <td><code>String</code></td>
            <td><p><em>1024 символа</em> </p></td>
        </tr><tr>
            <td><code>banners_count</code></td>
            <td><code>Integer</code></td>
            <td><p><br />Число баннеров в кампании</p></td>
        </tr><tr>
            <td><code>group_members</code></td>
            <td><code>String</code></td>
            <td><p><em>255 символов</em> </p></td>
        </tr><tr>
            <td><code>autobidding_mode</code></td>
            <td><code>String</code></td>
            <td><p><em>255 символов</em> <em>fixed, second_price, second_price_mean</em><br />Аукционная стратегия</p></td>
        </tr><tr>
            <td><code>append_utm</code></td>
            <td><code>Boolean</code></td>
            <td></td>
        </tr><tr>
            <td><code>age_restrictions</code></td>
            <td><code>String</code></td>
            <td><p><em>255 символов</em> <em>, 0+, 6+, 12+, 16+, 18+</em><br />Возрастные ограничения</p></td>
        </tr><tr>
            <td><code>slider_positions</code></td>
            <td><code></code></td>
            <td></td>
        </tr><tr>
            <td><code>last_updated</code></td>
            <td><code>DateTime</code></td>
            <td><p><br />Время последнего изменения (включая баннеры)</p></td>
        </tr><tr>
            <td><code>extended_age</code></td>
            <td><code>Boolean</code></td>
            <td></td>
        </tr><tr>
            <td><code>extended_pads</code></td>
            <td><code>Boolean</code></td>
            <td><p><br />Транслировать объявления на всех площадках подходящего формата (по умолчанию включено)</p></td>
        </tr><tr>
            <td><code>banners</code></td>
            <td><code>[BannerStats](bannerstat)</code></td>
            <td></td>
        </tr><tr>
            <td><code>stats</code></td>
            <td><code>[PeriodStat](periodstat)</code></td>
            <td><p><br />Статистика за всё время</p></td>
        </tr><tr>
            <td><code>stats_today</code></td>
            <td><code>[PeriodStat](periodstat)</code></td>
            <td><p><br />Статистика за сегодняшний день</p></td>
        </tr><tr>
            <td><code>stats_yesterday</code></td>
            <td><code>[PeriodStat](periodstat)</code></td>
            <td><p><br />Статистика за вчерашний день</p></td>
        </tr><tr>
            <td><code>stats_full</code></td>
            <td><code>[PeriodStats](periodstat)</code></td>
            <td><p><br />Статистика за последние 2 недели</p></td>
        </tr><tr>
            <td><code>last_stats_updated</code></td>
            <td><code>DateTime</code></td>
            <td><p><br />Время последней статистики</p></td>
        </tr>
    </tbody>
</table>