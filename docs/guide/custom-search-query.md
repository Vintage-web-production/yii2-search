Custom search query in model
============================

You can create custom query for search in specific model.

For it you should implement `\vintage\search\interfaces\CustomSearchInterface` interface
instead of `\vintage\search\interfaces\SearchInterface`

```php
/**
 * Home static page search model.
 * 
 * @property integer $id
 * @property string $slug
 * @property string $description
 */
class ArticleSearch extends ActiveRecord implements \vintage\search\interfaces\CustomSearchInterface
{
    /**
     * @inheritdoc
     */
    public function getSearchTitle()
    {
        return 'Home page';
    }

    /**
     * @inheritdoc
     */
    public function getSearchDescription()
    {
        return $this->description;
    }

    /**
     * @inheritdoc
     */
    public function getSearchUrl()
    {
       return '/home';
    }

   /**
    * @inheritdoc
    */
    public function getSearchFields()
    {
        return [
            'description',
        ];
    }
    
    /**
     * @inheritdoc
     */
    public function getQuery(ActiveQueryInterface $query, $field, $searchQuery)
    {
        return $query->orWhere([
            'and',
            ['like', $field, $searchQuery],
            ['slug' => 'home']
        ]);
    }
}
```

this interface provides `getQuery()` method.

It takes as arguments `ActiveQuery` object, current search field and search query string. You can build a custom query.

In method implementation you should return a `ActiveQuery` object.