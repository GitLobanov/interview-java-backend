Паттерн Sidecar предлагает помещать периферийные задачи, связанные с мониторингом, безопасностью, отказоустойчивостью и так далее, в отдельный компонент и развертывать его внутри собственного процесса или контейнера. Так обеспечивается однородный интерфейс для сервисов основного приложения, которые могут быть написаны на разных языках.

Sidecar не обязательно является частью приложения, но связан с ним: для каждого экземпляра приложения рядом развертывается экземпляр Sidecar. Sidecar имеет тот же жизненный цикл, что и основное приложение.

Преимуществами паттерна являются независимость вспомогательного компонента от платформы основного приложения, возможность их доступа к одним и тем же ресурсам, минимизация задержек из-за их близкого расположения и возможность независимого обновления. К недостаткам можно отнести накладные расходы на создание дополнительного компонента. Шаблон не рекомендуется использовать для небольших приложений, а также в тех случаях, когда можно обойтись библиотеками и стандартными механизмами расширений.

![[../../../../_res/Pasted image 20241215104804.png]]

