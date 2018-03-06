```py
'''
班主任测评
'''
import time
from selenium import webdriver


def mymain(name, password):
    driver = webdriver.Chrome()
    try:
        try:
            driver.get('http://stu.1000phone.net/student.php/Public/login')
            driver.find_element_by_name('Account').send_keys(name)
            driver.find_element_by_name('PassWord').send_keys(password + '\n')
            driver.find_element_by_link_text('学员评价').click()
            driver.find_element_by_link_text('开始评价').click()
            n = 0
            for label in driver.find_elements_by_tag_name('label'):
                n = n + 1
                print(label.text, n)
                if n % 4 == 1:
                    label.click()

            driver.find_element(value='NakdEA').send_keys('好好好  \n--来自上古遗留的bug的自动评价脚本')
            driver.find_element_by_id('addstudent').click()
            time.sleep(10)
        except:
            pass

        try:
            driver.find_element_by_link_text('开始评价').click()
        except:
            pass
        try:
            n = 0
            for label in driver.find_elements_by_tag_name('label'):
                n = n + 1
                print(label.text, n)
                if n % 4 == 1:
                    label.click()
            driver.find_element_by_id('fq3hTy').send_keys('好好好  \n--来自上古遗留的bug的自动评价脚本')
            driver.find_element_by_id('3v4JAF').send_keys('好好好  \n--来自上古遗留的bug的自动评价脚本')
            driver.find_element_by_id('addstudent').click()
        except:
            pass

    except Exception as e:
        print(e)
    driver.close()


if __name__ == '__main__':
    mymain(input("输入身份证号"), input("输入密码（默认身份证后六位）"))

```



