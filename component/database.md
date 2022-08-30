# 说明
数据库组件是基于`gorm`实现的，现已升级到2.0版本，支持读写分离与多数据库连接。


## 初始化
一般加载数据库是在main.go里的init函数，连接数据库放在加载配置项成功后。

- Demo
```go
package main
type User struct {
	Id   int
	Name string
}
func (u *User) TableName() string {
	return "users"
}
type OtherUser struct {
	Id   int
	Name string
}
func (u *OtherUser) TableName() string {
	return "other_users"
}
func TestComponentGorm(t *testing.T) {
	// test primary connection
	dsn := "root:123456@tcp(127.0.0.1:3306)/db1?charset=utf8mb4&parseTime=True&loc=Local"
	err := component.Provider().Gorm().OpenMySQL(dsn)
	assert.Nil(t, err)

	// test insert
	insertErr := component.Provider().Gorm().Create(&User{Name: "John"}).Error
	assert.Nil(t, insertErr)

	// use other connection
	otherDsn := "root:123456@tcp(127.0.0.1:3306)/db2?charset=utf8mb4&parseTime=True&loc=Local"
	resolverErr := component.Provider().Gorm().RegisterResolverConfig(dbresolver.Config{
		Sources: []gorm.Dialector{mysql.Open(otherDsn)},
	}, "other_users")
	assert.Nil(t, resolverErr)

	// insert other users with db2 connection
	otherInsertErr := component.Provider().Gorm().Create(&OtherUser{Name: "Tom"}).Error
	assert.Nil(t, otherInsertErr)

	// test insert primary connection
	insertPrimaryErr := component.Provider().Gorm().Create(&User{Name: "John"}).Error
	assert.Nil(t, insertPrimaryErr)
}
```
