---
title: Testing the Form Post in Spring Boot/MVC
description: Testing APIs in Spring Boot/MVC
date: 2017-08-01
draft: false
toc: false
tags:
  - testing
  - spring boot
  - mvc
---

Spring MVC (with its MockMVC) makes it super easy to test the controller and the wiring.
Here's a simple setup to test a Form POST.

### Controller
~~~ java
@Controller
public class PingController {

    @PostMapping("/ping")
    public String transactionComplete(@ModelAttribute Ping response) {
        return "pong"; //template name
    }
}
~~~

### Model
~~~ java
public class Ping {
    private String message = "";

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
~~~

### Template
~~~ html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>PING</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>

    <h1 th:text="${ping.message == ''} ? 'No message rcvd' : 'Message: ' + ${ping.message}" />
</body>
</html>
~~~

### Integration Test
~~~ java
@RunWith(SpringRunner.class)
@WebMvcTest(PingController.class)
public class PingControllerIntegrationTest {
    private static final String MESSAGE = "'sup yo?";

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void endpoint_exists() throws Exception {
        this.mockMvc.perform(post("/ping")).
                andExpect(status().isOk());
    }

    @Test
    public void attribute_value_is_mapped_correctly() throws Exception {
        this.mockMvc.perform(post("/ping")
                    .contentType(MediaType.APPLICATION_FORM_URLENCODED)
                    .content(encode("Message", "UTF-8") + "=" + encode(MESSAGE, "UTF-8")))
                .andExpect(status().isOk())
                .andExpect(model().attribute("ping", new BaseMatcher<Ping>() {
                    @Override
                    public boolean matches(Object response) {
                        return MESSAGE.equals(((Ping)response).getMessage());
                    }

                    @Override
                    public void describeTo(Description description) {
                    }
                }));
    }

    @Test
    public void page_is_rendered_with_message() throws Exception {
        this.mockMvc.perform(post("/ping")
                    .contentType(MediaType.APPLICATION_FORM_URLENCODED)
                    .content(encode("Message", "UTF-8") + "=" + encode(MESSAGE, "UTF-8")))
                .andExpect(status().isOk())
                .andExpect(content().string(new BaseMatcher<String>() {
                    @Override
                    public boolean matches(Object item) {
                        return ((String) item).contains("<h1>Message: &#39;sup yo?</h1>");
                    }

                    @Override
                    public void describeTo(Description description) {
                    }
                }));
    }

    @Test
    public void page_is_rendered_without_message() throws Exception {
        this.mockMvc.perform(post("/ping")
                    .contentType(MediaType.APPLICATION_FORM_URLENCODED))
                .andExpect(status().isOk())
                .andExpect(content().string(new BaseMatcher<String>() {
                    @Override
                    public boolean matches(Object item) {
                        return ((String) item).contains("<h1>No message rcvd</h1>");
                    }

                    @Override
                    public void describeTo(Description description) {
                    }
                }));
    }
}
~~~