НАЦІОНАЛЬНИЙ ТЕХНІЧНИЙ УНІВЕРСИТЕТ УКРАЇНИ 

«КИЇВСЬКИЙ ПОЛІТЕХНІЧНИЙ ІНСТИТУТ»

ФАКУЛЬТЕТ ІНФОРМАТИКИ І ОБЧИСЛЮВАЛЬНОЇ ТЕХНІКИ

КАФЕДРА ОБЧИСЛЮВАЛЬНОЇ ТЕХНІКИ








Лабораторна робота №6

з дисципліни «Архітектура комп’ютерів 2»








Виконала:

студентка 3 курсу 

групи ІВ-82

Чуясова Тетяна

Перевірив: 

Нікольський С. С.












Київ 2020 р.

Лістинг програми:

hello.c 

/*
 * Copyright (c) 2017, GlobalLogic Ukraine LLC
 * All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions are met:
 * 1. Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 * 2. Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 * 3. All advertising materials mentioning features or use of this software
 *    must display the following acknowledgement:
 *    This product includes software developed by the GlobalLogic.
 * 4. Neither the name of the GlobalLogic nor the
 *    names of its contributors may be used to endorse or promote products
 *    derived from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY GLOBALLOGIC UKRAINE LLC `AS IS` AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
 * WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
 * DISCLAIMED. IN NO EVENT SHALL GLOBALLOGIC UKRAINE LLC BE LIABLE FOR ANY
 * DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
 * (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
 * LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
 * ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
 * SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */
 
#include <linux/init.h>

#include <linux/module.h>

#include <linux/moduleparam.h>

#include <linux/printk.h>

#include <linux/types.h>

#include <linux/ktime.h>

#include <linux/slab.h>

struct head_list {

  struct head_list *next;
  
  ktime_t start_time;
  
};

MODULE_AUTHOR("Chuiasova Tetiana IV-82");

MODULE_DESCRIPTION("Hello world (Linux module for lab6)");

MODULE_LICENSE("Dual BSD/GPL");

static struct head_list *head;

static uint repeat = 1;

module_param(repeat, uint, 0444);

MODULE_PARM_DESC(repeat, "repeat amount");

static int __init hello_init(void)

{

  uint i = 0;
  
  struct head_list *this_var, *next_var;
  
  if (!repeat) {
  
    pr_info("Repetition parameter = 0");
    
    pr_info("");
    
    return 0;
    
  }  if (repeat >= 5 && repeat <= 10) {
  
    pr_warn("Repetition parameter is between 5 and 10");
    
  } else {    if (repeat > 10) {
  
      pr_err("Repetition parameter > 10");
      
      pr_info("");
      
      return -EINVAL;    }
      
  }
  head = kmalloc(sizeof(struct head_list *), GFP_KERNEL);
  
  this_var = head;
  
  for (i = 0; i < repeat; i++) {
  
    this_var->next = kmalloc(sizeof(struct head_list), GFP_KERNEL);
    
    this_var->start_time = ktime_get();
    
    
    pr_info("Hello, world!\n");
    
    next_var = this_var;
    
    this_var = this_var->next;
    
  }
  pr_info("Repeat: %d\n", repeat);
  
  kfree(next_var->next);
  
  next_var->next = NULL;
  
  return 0;
}

static void __exit hello_exit(void)

{
  struct head_list *tmp_var;
  
  while (head != NULL && repeat != 0) {
  
    tmp_var = head;
    
    pr_info("Time of printing: %lld", tmp_var->start_time);
    
    head = tmp_var->next;
    
    kfree(tmp_var);
    
  }
  
  pr_info("");
  
}

module_init(hello_init);

module_exit(hello_exit);


Посилання на репозиторій:

https://github.com/txt2021/Lab6_AK







Результати виконання роботи: (Скріншоти знаходяться у файлі звіту)

 
 
