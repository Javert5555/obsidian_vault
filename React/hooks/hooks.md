# useEffect
```
    useEffect(() => {

        const abortController = new AbortController()

        const signal = abortController.signal

        // or use 'then' here

        const data = await list(signal)

        if (data && data.error) {

            console.log(data.error)

        } else {

            setUsers(data)

        }

  

        return () => {

            abortController.abort()

        }

    }, [])

}
```

В этом эффекте мы также добавляем функцию очистки, чтобы прервать вызов выборки, когда компонент размонтируется. Чтобы связать сигнал с вызовом выборки, мы используем AbortController web API, который позволяет нам прерывать DOM-запросы по мере необходимости.

Во втором аргументе этого хука useEffect мы передаем пустой массив, чтобы очистка эффекта выполнялась только один раз при монтировании и размонтировании, а не после каждого рендеринга.